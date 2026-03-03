# moltis-stuff

Shared Moltis skills, documentation, and tools.

**Official Moltis Repository**: https://github.com/moltis-org/moltis

## 🇨🇳 Chinese Documentation

本项目提供完整的中文文档，包含所有功能介绍和使用说明。

👉 查看中文版： [docs/README_zh.md](docs/README_zh.md)

---

## 📖 Contents

- **skills/** - Reusable Moltis skill definitions
- **docs/** - Guides and reference material  
- **tools/** - Helper scripts and utilities

## 🔧 Skills

### search-exa

Exa AI search engine integration for intelligent web search (curl-based)

- Semantic search capabilities with advanced filtering
- Requires `EXA_API_KEY` environment variable
- Uses tool: `exec`

### moltis-updater

Secure version update tool for Moltis

- Two-phase update workflow (download & verify → independent execution)
- Automatic version detection and comparison
- Uses `systemd-run` to run update script independently, avoiding service interruption

## 🚀 Usage

Skills in this repository can be imported into your Moltis installation.

## 🤝 Contributing

Feel free to submit issues and pull requests!
