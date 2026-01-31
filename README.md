# 知炼 (ZhiForge)

将知识库炼化为知乎回答的 Claude Code Skill。

## 功能

- `/check-zhihu` - 查看知乎邀请并匹配博客
- `/draft-answer` - 草拟知乎回答
- `/publish-zhihu` - 发布知乎文章
- `/auto-zhihu` - 自动知乎回答（全流程）

## 前置依赖：Chrome MCP

本 Skill 需要 Chrome MCP Server 来操作知乎页面。

### 1. 安装 Chrome MCP

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

### 3. 启动 Chrome（带调试端口）

```bash
# macOS
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222

# Windows
chrome.exe --remote-debugging-port=9222

# Linux
google-chrome --remote-debugging-port=9222
```

### 4. 登录知乎

在启动的 Chrome 中登录你的知乎账号。

---

## 安装 Skill

### 方式一：自动安装（推荐）

在 Claude Code 中运行：

```
/install-github-skill yfge/zhiforge
```

### 方式二：手动安装

```bash
git clone https://github.com/yfge/zhiforge.git ~/.claude/skills/zhiforge
```

---

## 配置

安装后需要配置知识库路径和作者信息。

### 知识库目录

编辑 `~/.claude/skills/zhiforge/.claude/settings.json`：

```json
{
  "knowledge_base": "/path/to/your/markdown/files",
  "blog_url": "https://your-blog.github.io",
  "author": {
    "zhihu_name": "你的知乎昵称",
    "wechat_official": "你的公众号",
    "bio": "你的简介"
  },
  "expertise": ["领域1", "领域2"],
  "high_match_topics": ["话题1", "话题2"]
}
```

### 配置模板

首次使用可从模板复制：

```bash
cp ~/.claude/skills/zhiforge/.claude/settings.example.json \
   ~/.claude/skills/zhiforge/.claude/settings.json
```

---

## 使用

确保 Chrome MCP 已启动且知乎已登录，然后：

```
/auto-zhihu
```

即可自动完成：搜索问题 → 匹配知识库 → 撰写文章 → 发布回答。

---

## 许可证

MIT
