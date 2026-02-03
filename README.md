# 知炼 (ZhiForge)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code Skill](https://img.shields.io/badge/Claude_Code-Skill-blue)](https://docs.anthropic.com/en/docs/build-with-claude/claude-code)
[![GitHub stars](https://img.shields.io/github/stars/yfge/zhiforge)](https://github.com/yfge/zhiforge/stargazers)

> 将知识库炼化为知乎回答的 Claude Code Skill

**知炼**是一个基于 Claude Code 的自动化工具，能够将你的 Markdown 知识库（博客文章）智能转化为高质量的知乎回答，并自动发布。

## ✨ 特性

- 🤖 **全自动流程**：搜索热点问题 → 匹配知识库 → 撰写文章 → 发布回答
- 📚 **知识库驱动**：基于你的博客文章/Markdown 文档智能生成回答
- 🎯 **智能匹配**：自动评估问题与知识库的匹配度
- 🚀 **一键发布**：通过 Chrome MCP 自动化操作知乎页面
- 📝 **保持风格**：维持作者的写作风格和专业领域

## 🎬 快速开始

### 前置要求

- [Claude Code CLI](https://docs.anthropic.com/en/docs/build-with-claude/claude-code)
- Node.js 18+
- Chrome 浏览器
- 知乎账号

### 1. 安装 Chrome MCP Server

Chrome MCP 用于自动化操作知乎页面（登录、填写、发布）。

```bash
npm install -g @anthropic/claude-code-mcp-chrome-devtools
```

### 2. 配置 Claude Code

编辑 `~/.claude/mcp.json`，添加：

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["@anthropic/claude-code-mcp-chrome-devtools"]
    }
  }
}
```

### 3. 安装 ZhiForge Skill

#### 方式一：自动安装（推荐）

在 Claude Code 中运行：

```
/install-github-skill yfge/zhiforge
```

#### 方式二：手动克隆

```bash
git clone https://github.com/yfge/zhiforge.git ~/.claude/skills/zhiforge
```

### 4. 配置知识库

复制示例配置并编辑：

```bash
cp ~/.claude/skills/zhiforge/.claude/settings.example.json \
   ~/.claude/skills/zhiforge/.claude/settings.json
```

编辑 `~/.claude/skills/zhiforge/.claude/settings.json`：

```json
{
  "knowledge_base": "/path/to/your/blog/_posts",
  "blog_url": "https://yourblog.github.io",
  "author": {
    "zhihu_name": "你的知乎昵称",
    "wechat_official": "你的公众号",
    "bio": "你的简介"
  },
  "expertise": [
    "AI 编程",
    "Claude Code",
    "开源项目"
  ],
  "high_match_topics": [
    "vibe-coding",
    "AI 辅助开发",
    "技术选型"
  ]
}
```

### 5. 启动 Chrome 并登录知乎

```bash
# macOS
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222

# Windows
chrome.exe --remote-debugging-port=9222

# Linux
google-chrome --remote-debugging-port=9222
```

在打开的 Chrome 中登录你的知乎账号。

### 6. 开始使用

在 Claude Code 中运行：

```
/auto-zhihu
```

或者使用其他命令：

```
/check-zhihu      # 查看知乎邀请并匹配知识库
/draft-answer     # 草拟知乎回答
/publish-zhihu    # 发布知乎文章
/hot-zhihu        # 搜索热点问题并自动回答
```

## 📖 使用场景

### 场景 1：自动回答知乎邀请

你收到了一些知乎问题邀请，想基于自己的博客文章快速生成高质量回答：

```
/check-zhihu
```

ZhiForge 会：
1. 检查你的知乎邀请列表
2. 分析每个问题与你知识库的匹配度
3. 自动草拟回答并发布

### 场景 2：主动搜索热点问题

你想找到与你专业领域相关的热门问题，并发表观点：

```
/hot-zhihu
```

ZhiForge 会：
1. 搜索知乎热点问题（基于你的专业领域）
2. 评估匹配度
3. 撰写并发布回答

### 场景 3：手动控制流程

你想更精细地控制每个步骤：

```
1. /check-zhihu      # 先查看匹配情况
2. /draft-answer     # 草拟回答
3. /publish-zhihu    # 确认后发布
```

## 🗂️ 知识库组织

ZhiForge 支持标准的 Jekyll 博客结构：

```
your-blog/
├── _posts/
│   ├── 2025-01-15-ai-programming.md
│   ├── 2025-01-20-docker-guide.md
│   └── 2025-01-25-git-workflow.md
└── ...
```

每篇文章的元数据会被自动提取，用于智能匹配：

```markdown
---
layout: post
title: AI 编程实战指南
tags: [AI, Claude Code, 编程]
---

# AI 编程实战指南

文章内容...
```

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

1. Fork 本仓库
2. 创建你的特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交你的改动 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启一个 Pull Request

## 📝 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件

## 🙏 致谢

- [Claude Code](https://docs.anthropic.com/en/docs/build-with-claude/claude-code) - 强大的 AI 编程助手
- [Chrome MCP](https://github.com/anthropics/claude-code-mcp-chrome-devtools) - 浏览器自动化工具

## 📮 联系方式

- 作者：[yfge](https://github.com/yfge)
- 博客：[https://yfge.github.io](https://yfge.github.io)

---

如果这个项目对你有帮助，欢迎 ⭐ Star！
