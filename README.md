# moltis-stuff

Shared Moltis skills, documentation, and tools.

## Contents

- **skills/** - Reusable Moltis skill definitions
- **docs/** - Guides and reference material
- **tools/** - Helper scripts and utilities

## Skills

### search-exa
使用 Exa AI 搜索引擎进行智能网络搜索（基于 curl）
- 支持语义搜索、高级过滤
- 需要 `EXA_API_KEY` 环境变量
- 使用 tool: `exec`

### moltis-updater
Moltis 安全版本更新工具
- 两阶段更新流程（下载验证 + 独立执行）
- 自动版本检测与比较
- 通过 systemd-run 后台执行，避免服务中断

## Usage

Skills in this repository can be imported into your Moltis installation.

## Contributing

Feel free to submit issues and pull requests!
