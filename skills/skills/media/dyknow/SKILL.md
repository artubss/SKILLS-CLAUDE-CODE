---
name: dyknow
description: 抖音视频转本地知识库 Markdown 笔记。下载抖音视频、语音转录文字、生成 Obsidian 兼容笔记，支持批量同步收藏夹和关键词搜索浏览。Use when users want to save Douyin/TikTok videos as knowledge base notes, transcribe video speech to text, sync favorites, or build a video knowledge library.
---

# DyKnow — 抖音视频 → 知识库

将抖音视频自动化转化为本地 Markdown 知识库笔记，支持视频下载、语音转录、AI 摘要和自动归类。

## 前置条件

```bash
# 1. 安装依赖
pip install -r requirements.txt
playwright install chromium

# 2. 下载转录模型（二选一）
#    ggml-tiny.bin (74MB，推荐) 或 ggml-small.bin (465MB)
#    放入 data/models/ 目录

# 3. 扫码登录抖音（Cookie 持久化）
python -m dyknow login
```

## 三种使用场景

### 场景 A：单视频解析

当用户在聊天中粘贴含抖音链接的分享文案时：

```
python -m dyknow parse "7.64 03/20 https://v.douyin.com/xxx/ ..."
```

AI 会读取生成的 MD 笔记 → 生成摘要 → 自动归类 → 更新索引。

### 场景 B：批量同步收藏

当用户说"同步我的收藏"或"备份抖音内容"时：

```bash
python -m dyknow sync                    # 增量同步元数据索引
python -m dyknow sync --transcribe       # 同步 + 下载视频 + 转录
python -m dyknow sync --source collection  # 仅收藏夹精选
```

**注意**：转录耗时 1-3 分钟/条，需提前告知用户。

### 场景 C：主题浏览/研究

当用户说"帮我搜 AI 工具相关的抖音视频"时：

```bash
python -m dyknow browse -k "AI工具" --count 30          # 索引
python -m dyknow browse -k "AI工具" --count 30 --transcribe  # +转录
```

## 输出结构

```
<输出目录>/
├── 未分类/              ← 原始下载，等待 AI 处理
└── <知识库名称>/        ← AI 已处理，按主题自动归类
    ├── 01-AI工具与开发/
    ├── 03-自媒体与运营/
    └── ...
```

每条视频生成 Obsidian 兼容的 Markdown 笔记：

```markdown
---
title: "AI工具推荐"
video_id: "7234567890123456789"
source: "https://www.douyin.com/video/7234567890123456789"
date: 2026-07-05
author: "科技博主"
status: done
---

# AI工具推荐

> [!info] 视频信息
> - 时长: 03:25 | 👍 12,345

## 📝 转录文本
...

## 🤖 AI 摘要
### 一句话总结
...
### 要点
- ...
### 金句
- "..."
```

## 可用命令速查

| 命令 | 用途 |
|------|------|
| `login` | 扫码登录抖音 |
| `status` | 查看登录+同步状态 |
| `parse "<文本>"` | 单视频解析 |
| `parse "<文本>" --no-transcribe` | 仅元数据 |
| `sync` | 增量同步索引 |
| `sync --transcribe` | 同步+转录 |
| `sync --source collection` | 收藏夹精选 |
| `browse -k "关键词"` | 主题浏览 |
| `browse -k "关键词" --transcribe` | 浏览+转录 |
| `transcribe` | 断点续转 |
| `transcribe --retry-failed` | 重试失败项 |
| `night` | 夜间批量转录 |

## 核心依赖

- Python 3.11+, requests, playwright
- pywhispercpp (ggml-tiny 74MB / ggml-small 465MB，CPU/GPU 通用)
- AI 摘要由调用方模型完成，不依赖外部 API

## 更多

- 完整文档：https://github.com/stank2386322761/dyknow
- 设计文档：https://github.com/stank2386322761/dyknow/blob/master/DESIGN.md
