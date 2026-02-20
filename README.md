# 知炼 (ZhiForge)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code Skill](https://img.shields.io/badge/Claude_Code-Skill-blue)](https://docs.anthropic.com/en/docs/build-with-claude/claude-code)
[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-green)](https://docs.openclaw.ai)
[![GitHub stars](https://img.shields.io/github/stars/yfge/zhiforge)](https://github.com/yfge/zhiforge/stargazers)

> 将知识库炼化为知乎回答的自动化 Skill，支持 Claude Code 和 OpenClaw

**知炼**是一个自动化工具，能够将你的 Markdown 知识库（博客文章）智能转化为高质量的知乎回答，并自动发布。

## ✨ 特性

- 🤖 **全自动流程**：搜索热点问题 → 去重 → 匹配知识库 → 撰写文章 → 发布回答
- 📚 **知识库驱动**：基于你的博客文章/Markdown 文档智能生成回答
- 🎯 **智能匹配**：自动评估问题与知识库的匹配度
- 🚀 **一键发布**：自动化操作知乎页面
- 📝 **保持风格**：维持作者的写作风格和专业领域
- 🔄 **闭环回存**：发布后的文章自动回存到知识库（OpenClaw）
- ✅ **质量审核**：多模型交叉验证文章质量（OpenClaw）

## 🎬 快速开始

### 方式一：OpenClaw（推荐）

OpenClaw 内置 browser 工具，无需额外安装 Chrome MCP。

#### 1. 安装

```bash
# 克隆到 OpenClaw workspace 的 skills 目录
git clone https://github.com/yfge/zhiforge.git <workspace>/skills/zhiforge
```

OpenClaw 会自动加载 `skills/` 下包含 `SKILL.md` 的目录。

#### 2. 配置

编辑 `<workspace>/skills/zhiforge/.claude/settings.json`：

```json
{
  "knowledge_base": "/path/to/your/blog/_posts",
  "blog_url": "https://yourblog.github.io",
  "blog_repo": "/path/to/your/blog/repo",
  "author": {
    "zhihu_name": "你的知乎昵称",
    "wechat_official": "你的公众号",
    "bio": "你的简介"
  },
  "expertise": ["AI 编程", "Claude Code"],
  "high_match_topics": ["vibe-coding", "AI 辅助开发"]
}
```

#### 3. 使用

在 OpenClaw 中发送：

```
自动回答知乎
```

或者：

```
检查知乎邀请
草拟知乎回答 [问题标题]
知乎热点
```

#### OpenClaw 完整流程（9 步闭环）

1. **读取配置** → 知识库路径、作者信息
2. **获取已回答列表** → 去重
3. **搜索热点问题** → 邀请回答 + 关键词搜索
4. **匹配知识库** → 找相关博客文章
5. **撰写文章** → 保持作者风格
6. **发布到知乎** → 专栏页面导入 Markdown + 关联问题
7. **质量审核** → 多模型交叉验证
8. **回存知识库** → Jekyll frontmatter + git push
9. **输出报告** → 链接、评分、匹配博客

详细流程见 [SKILL.md](SKILL.md)。

---

### 方式二：Claude Code

#### 前置要求

- [Claude Code CLI](https://docs.anthropic.com/en/docs/build-with-claude/claude-code)
- Node.js 18+
- Chrome 浏览器（需安装 Chrome MCP）
- 知乎账号

#### 1. 安装 Chrome MCP Server

```bash
npm install -g @anthropic/claude-code-mcp-chrome-devtools
```

编辑 `~/.claude/mcp.json`：

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

#### 2. 安装 ZhiForge

```bash
# 自动安装
/install-github-skill yfge/zhiforge

# 或手动克隆
git clone https://github.com/yfge/zhiforge.git ~/.claude/skills/zhiforge
```

#### 3. 配置

```bash
cp ~/.claude/skills/zhiforge/.claude/settings.example.json \
   ~/.claude/skills/zhiforge/.claude/settings.json
# 编辑 settings.json 填入你的配置
```

#### 4. 启动 Chrome 并登录知乎

```bash
# macOS
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222
```

#### 5. 使用

```
/auto-zhihu        # 全自动流程
/check-zhihu       # 查看知乎邀请并匹配知识库
/draft-answer      # 草拟知乎回答
/publish-zhihu     # 发布知乎文章
/hot-zhihu         # 搜索热点问题并自动回答
```

## 🗂️ 知识库组织

支持标准 Jekyll 博客结构：

```
your-blog/
├── _posts/
│   ├── 2025-01-15-ai-programming.md
│   ├── 2025-01-20-docker-guide.md
│   └── 2025-01-25-git-workflow.md
└── ...
```

## 🏗️ 项目结构

```
zhiforge/
├── SKILL.md                    # OpenClaw Skill 定义（AgentSkills 格式）
├── CLAUDE.md                   # Claude Code 项目说明 + 博客索引
├── README.md
├── LICENSE
└── .claude/
    ├── settings.json           # 配置文件（需自行编辑）
    ├── settings.example.json   # 配置示例
    └── commands/
        ├── auto-zhihu.md       # 全自动流程
        ├── hot-zhihu.md        # 热点搜索流程
        ├── check-zhihu.md      # 检查邀请流程
        ├── draft-answer.md     # 草拟回答
        └── publish-zhihu.md    # 发布流程
```

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📝 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件

## 🙏 致谢

- [Claude Code](https://docs.anthropic.com/en/docs/build-with-claude/claude-code) - AI 编程助手
- [OpenClaw](https://github.com/openclaw/openclaw) - 开源 AI Agent 平台
- [Chrome MCP](https://github.com/anthropics/claude-code-mcp-chrome-devtools) - 浏览器自动化

## 📮 联系方式

- 作者：[yfge](https://github.com/yfge)
- 博客：[https://yfge.github.io](https://yfge.github.io)

---

如果这个项目对你有帮助，欢迎 ⭐ Star！
