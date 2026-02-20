---
name: zhiforge
description: 知炼 - 将知识库(Markdown博客)智能转化为知乎回答并自动发布。支持搜索热点问题、匹配知识库、撰写文章、自动发布、质量审核、回存知识库的完整闭环。
user-invocable: true
metadata: {"openclaw": {"emoji": "🔥"}}
---

# 知炼 (ZhiForge) — 知识库 → 知乎回答 完整闭环

## 配置

配置文件：`{baseDir}/.claude/settings.json`

```json
{
  "knowledge_base": "/path/to/markdown/files",
  "blog_url": "https://your-blog.github.io",
  "blog_repo": "/path/to/blog/git/repo",
  "author": {
    "zhihu_name": "你的知乎昵称",
    "wechat_official": "你的公众号",
    "bio": "你的简介"
  },
  "expertise": ["AI 编程", "Claude Code"],
  "high_match_topics": ["vibe-coding", "AI 辅助开发"]
}
```

## 完整流程（全自动，不问用户）

当用户说"知乎回答"、"zhiforge"、"自动回答知乎"等触发词时，执行以下完整流程：

### Step 1: 读取配置
1. 读取 `{baseDir}/.claude/settings.json` 获取知识库路径、作者信息
2. 读取 `{baseDir}/CLAUDE.md` 获取博客文章索引

### Step 2: 获取已回答列表（去重）
1. 用 browser 打开 `https://www.zhihu.com/creator/manage/creation/article`
2. 获取已发布文章标题列表
3. 也检查回答列表：`https://www.zhihu.com/creator/manage/creation/answer`

### Step 3: 搜索热点问题
两种来源：
- **邀请回答**：`https://www.zhihu.com/creator/featured-question/invited`
- **搜索热点**：用专业领域关键词搜索 `https://www.zhihu.com/search?type=content&q=...`

筛选标准：
- 与知识库匹配度高（AI编程/vibe-coding/Claude Code 等）
- 回答数少、关注数多的优先
- 排除已回答过的（标题相似度 > 80%）

### Step 4: 匹配知识库
1. 搜索 knowledge_base 目录中的 .md 文件
2. 根据问题关键词匹配相关文章
3. 读取匹配的博客内容作为素材
4. 匹配度低的跳过，找下一个问题

### Step 5: 撰写文章
写作风格（保持老拐风格）：
- 开门见山，先给结论
- 用实际案例说话（引用博客中的项目经验）
- 技术内容要落地可执行
- 代码块标注语言
- 重点用 **加粗**
- 结尾引导关注博客

将文章写入 `/tmp/zhihu-article.md`

### Step 6: 发布文章
**必须使用专栏页面发布**（避免反爬）：

1. 打开 `https://zhuanlan.zhihu.com/write`
2. 点击"导入" → "导入文档" → 上传 markdown 文件
3. 等待导入完成（正文出现内容）
4. 填写标题（在标题输入框中 type）
5. 点击"投稿至问题"下拉框 → 搜索目标问题关键词 → 点"选择" → 点"确定"
6. 点击"发布"按钮
7. 等待跳转（URL 变化表示成功）

**关键坑点：**
- 不能直接 fill 正文，markdown 语法会变纯文本 → 必须用导入
- 导入是追加不是替换 → 每次打开新的 write 页面
- 投稿至问题的搜索框是 combobox → 先 click 聚焦再输入
- 对话框用 Escape 关闭
- 发布按钮可能 disabled → 要等标题和正文都有内容

### Step 7: 质量审核（交叉验证）
发布后，用另一个模型（如 qwen）审核文章质量：
- 标题吸引力、开头力度、论据充实度
- 逻辑结构、知乎风格匹配、结尾引导
- 总分和改进建议

使用 `sessions_spawn` + 不同 model 参数来实现交叉验证。

### Step 8: 回存知识库（闭环）
如果质量审核通过：
1. 在文章开头加上 Jekyll frontmatter（layout、title、tags）
2. 保存到 `blog_repo/_posts/YYYY-MM-DD-标题.md`
3. `git add` + `git commit` + `git push`

这样知乎文章反向充实了博客知识库，形成正向循环。

### Step 9: 输出报告

```
✅ 发布成功

问题：[问题标题]
文章：[文章标题]
链接：https://zhuanlan.zhihu.com/p/xxxxx
参考博客：[博客文件名]
质量评分：[总分]/10
知识库：已回存 ✅
```

## Browser 工具映射

所有页面操作使用 OpenClaw 的 `browser` 工具（profile="openclaw"）：

| 操作 | 命令 |
|------|------|
| 打开页面 | `browser action=navigate targetUrl="..."` |
| 获取内容 | `browser action=snapshot compact=true` |
| 点击 | `browser action=act request={kind:"click", ref:"..."}` |
| 输入 | `browser action=act request={kind:"type", ref:"...", text:"..."}` |
| 按键 | `browser action=act request={kind:"press", key:"Escape"}` |
| 上传文件 | `browser action=upload paths=["/path/to/file"]` |
| 截图 | `browser action=screenshot` |
| 等待 | `browser action=act request={kind:"wait", timeMs:3000}` |

## 其他命令

- **"检查知乎邀请"**：只执行 Step 2-3，列出可回答问题
- **"草拟知乎回答 [问题]"**：只执行 Step 4-5，不发布
- **"知乎热点"**：搜索热点 + 全流程

详细命令模板见 `{baseDir}/.claude/commands/*.md`

## 注意事项

- **全程自动执行**，遇到问题自行决策或跳过
- **知识库匹配度低的问题要跳过**，不要强行回答
- 保持作者风格：务实、不油腻、有干货
- 发布失败重试一次，仍失败则输出错误信息
- 连续 3 个问题都不合适则结束
