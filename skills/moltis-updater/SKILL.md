---
name: moltis-updater
description: Moltis 安全版本更新工具，两阶段流程（下载解压验证 + 独立脚本执行）
---

# Moltis Updater Skill

## 概述

安全的两阶段自动更新 Moltis，避免服务中断。下载解压验证通过后，在独立 systemd 单元中执行停止-替换-重启。

## 核心流程

### 阶段一：准备阶段（moltis 进程内）
1. 获取当前版本
2. 从 GitHub API 获取最新版本
3. 比较版本号（语义化）
4. 如有更新：下载 → 解压 → 验证
5. 生成 update.sh 脚本到 `/tmp`
6. 通过 `systemd-run --no-block` 后台执行 update.sh
7. 退出

### 阶段二：执行阶段（独立 systemd 单元）
update.sh 内容：
```bash
#!/bin/bash
set -euo pipefail

# 停止服务
systemctl stop moltis

# 替换文件
cp /tmp/moltis-v0.10.11/amd64/moltis /usr/bin/moltis
chmod +x /usr/bin/moltis

# 重启服务
systemctl start moltis

# 验证
sleep 2
/usr/bin/moltis --version

# 清理临时文件
rm -rf /tmp/moltis-v0.10.11
rm -f /tmp/moltis-update.*.sh
```

## 实现细节

### 版本获取
```bash
# 当前版本
CURRENT=$(/usr/bin/moltis --version 2>/dev/null | head -1 | grep -oE '[0-9]+\.[0-9]+\.[0-9]+')

# GitHub 最新版本（修复变量名 bug）
LATEST_TAG=$(curl -s https://api.github.com/repos/moltis-org/moltis/releases/latest | jq -r '.tag_name')
LATEST_VERSION=${LATEST_TAG#v}
```

### 下载 URL
```bash
# x86_64
URL="https://github.com/moltis-org/moltis/releases/download/${LATEST_TAG}/moltis_${LATEST_VERSION}_Linux_x86_64.tar.zst"

# aarch64（备用）
URL_AARCH="https://github.com/moltis-org/moltis/releases/download/${LATEST_TAG}/moltis_${LATEST_VERSION}_Linux_arm64.tar.zst"
```

### 临时脚本生成
```bash
UPDATE_SCRIPT=$(mktemp /tmp/moltis-update.XXXXXX.sh)
cat > "$UPDATE_SCRIPT" << 'EOF'
#!/bin/bash
set -euo pipefail
# ... 阶段二脚本内容
EOF
chmod +x "$UPDATE_SCRIPT"

# 后台执行（与 moltis 进程解耦）
systemd-run --no-block --unit=moltis-update "$UPDATE_SCRIPT"
```

## 错误处理

- **下载失败**：abort，服务继续运行
- **解压失败**：清理临时文件，abort
- **启动失败**：update.sh 会因 `set -e` 退出，但不会自动回滚（需要手动从备份恢复，可扩展）

## 预检查

运行前确保：
- 有 `curl`、`jq`、`tar`、`systemctl` 可用
- root 权限
- systemd 系统

## 使用方法

在 moltis 中执行 `update` 命令，或直接调用此 skill 的自动更新逻辑。
