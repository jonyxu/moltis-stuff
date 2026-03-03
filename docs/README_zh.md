# moltis-stuff

共享的 Moltis 技能、指导文档和相关工具。

## 📖 目录内容

- **skills/** - 可复用的 Moltis 技能定义
- **docs/** - 指南和参考资料  
- **tools/** - 辅助脚本和实用工具

## 🔧 技能

### search-exa

集成 Exa AI 搜索引擎，实现智能网络搜索（基于 curl）

- 具备语义搜索能力和高级过滤功能
- 需要设置 `EXA_API_KEY` 环境变量
- 使用 tool: `exec`

### moltis-updater

Moltis 安全的版本更新工具

- 两阶段更新流程（下载并验证 → 独立执行）
- 自动检测和比较版本
- 使用 `systemd-run` 后台执行更新脚本，避免服务中断

## 🚀 使用方法

本仓库中的技能可以导入到你的 Moltis 安装中。

## 🤝 贡献

欢迎提交 issue 和 pull request！
