---
name: search-exa
description: 使用 Exa AI 搜索引擎进行智能网络搜索（基于 curl）
allowed_tools:
  - exec
---

# Exa AI 搜索技能（curl 实现版）

## 概述
使用 Exa AI 智能搜索引擎进行网络搜索，通过系统 curl 命令实现，支持语义搜索、相关内容查找等高级功能。

## 配置
必需环境变量：
- `EXA_API_KEY` - 你的 Exa API 密钥

获取地址：https://exa.ai

## 工具权限
- `exec` - 执行 curl 命令

## 使用方法

### 基础搜索
```python
from skill import exa_search

# 基础搜索
results = exa_search("伊朗局势最新动态")
```

### 高级选项
```python
# 带选项的搜索
results = exa_search(
    query="人工智能最新进展",
    num_results=10,
    include_text=True,
    use_autoprompt=False,
    type="neural"  # 或 "keyword"
)
```

## 参数说明
| 参数 | 类型 | 默认 | 说明 |
|------|------|------|------|
| query | str | 必填 | 搜索查询文本 |
| num_results | int | 10 | 结果数量（最大100） |
| include_text | bool | True | 是否包含网页正文 |
| use_autoprompt | bool | True | 是否自动优化查询 |
| type | str | "neural" | 搜索类型：neural/keyword |
| start_crawl_date | str | None | 爬取开始日期（YYYY-MM-DD）|
| end_crawl_date | str | None | 爬取结束日期（YYYY-MM-DD）|
| include_domains | list | None | 限制搜索的域名 |
| exclude_domains | list | None | 排除的域名 |
| use_cache | bool | True | 是否使用缓存 |

## 返回格式
```python
{
  "query": "原始查询",
  "autoprompt_string": "优化后的查询",
  "request_time": "2026-03-01T...",
  "total_results": 100,
  "auto_prompt": "自动提示词",
  "next_page": "下一页token",
  "search_response": {
    "results": [
      {
        "id": "unique_id",
        "url": "https://...",
        "title": "页面标题",
        "text": "网页正文内容...",
        "highlights": ["关键词片段1", "关键词片段2"],
        "score": 0.95
      }
    ]
  }
}
```

## 实现细节
使用 `curl` 命令调用 Exa API：
```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"...","numResults":10,...}'
```

响应通过 `response.json()` 解析。

## 示例代码
```python
import subprocess
import json
import os

def exa_search(query, **kwargs):
    """执行 Exa AI 搜索"""
    
    # 构建请求体
    payload = {"query": query, **kwargs}
    
    # curl 命令
    cmd = [
        "curl", "-X", "POST", "https://api.exa.ai/search",
        "-H", "x-api-key: " + os.environ["EXA_API_KEY"],
        "-H", "Content-Type: application/json",
        "-d", json.dumps(payload)
    ]
    
    # 执行
    result = subprocess.run(cmd, capture_output=True, text=True)
    
    if result.returncode != 0:
        raise Exception(f"Curl error: {result.stderr}")
    
    return json.loads(result.stdout)
```
