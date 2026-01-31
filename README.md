# 知炼 (ZhiForge)

将知识库炼化为知乎回答的 Claude Code Skill。

## 功能

- `/check-zhihu` - 查看知乎邀请并匹配博客
- `/draft-answer` - 草拟知乎回答
- `/publish-zhihu` - 发布知乎文章
- `/auto-zhihu` - 自动知乎回答（全流程）

## 配置

### 知识库目录

在 `.claude/settings.json` 中配置：

```json
{
  "knowledge_base": "/path/to/your/markdown/files",  // 存放 .md 文件的目录
  "blog_url": "https://your-blog.github.io"          // 可选，用于文章结尾引导
}
```

### 作者信息

在 `CLAUDE.md` 中配置：

- 知乎昵称
- 公众号名称
- 专业领域
- 博客文章索引

## 依赖

- Chrome MCP Server（用于操作知乎页面）
- 知乎账号已登录

## 安装

```bash
git clone https://github.com/yfge/zhiforge.git
cd zhiforge

# 复制配置模板
cp .claude/settings.example.json .claude/settings.json

# 编辑配置
vim .claude/settings.json
```

## 使用

1. 在 Claude Code 中打开项目目录
2. 运行 `/auto-zhihu` 开始自动回答流程

## 许可证

MIT
