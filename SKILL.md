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

#### 去 AI 味（核心，必须严格执行）

> AI味的本质不是某几个词。它是一种**过度的秩序感**——人类的文字是混乱的、偏执的、不完美的，AI的文字像一个穿着打扮完美但眼神空洞的人。

**五大根因（理解了就能自己发明去AI味技法）：**
1. **均值回归**：AI总生成"最可能的下一个词"，所有输出都朝平均值靠拢。人类写作有极端值——特别好或特别烂的句子共存。
2. **对称性**：AI爱对称结构（"不是A而是B""既有A也有B"）。人类偏向某一边，忽略另一边。
3. **情感正确性**：AI的情感总是"合理"的。人类的情感经常错位、矛盾、不成比例。
4. **无损信息传递**：AI每句话都携带信息，没有"废话"。人类写作充满废话——这些废话恰恰是"人味"核心。
5. **视角稳定性**：AI的叙述像上帝视角——观察力完美。人类有盲区、偏见、自我欺骗。

**必杀句式（出现即改，零容忍）：**
```
❌ "不是A，而是B" / "与其说A，不如说B" / "这不仅仅是A，更是B"
❌ "某种意义上来说" / "在这个A的时代" / "值得一提的是"
❌ "不得不说" / "毫无疑问" / "A，而非B"
❌ "事实上"作为段落开头 / "然而"作为段落开头（全文最多2次）
❌ "心中涌起一股难以言喻的……" / "一种说不清道不明的感觉"
❌ "XX如同XX一般，XX着XX"（隐喻+动名词的标准AI句式）
❌ "不禁……" / "某种程度上" / "那一瞬间"
```

**段落级检查：**
- **段落节奏均匀 = 最致命的AI指纹**。数每段句子数，如果80%都是3-5句 → AI。人类的段落分布不均匀，独句段应占 20-30%。
- **因果链太完整 = AI**。允许跑题、允许没有因果的并置、允许"死线索"。
- **信息分布太均匀 = AI**。有的段落信息密度高（一句话翻转整段），有的段落几乎没新信息。像鼓点，不像白噪音。

**全文级检查：**
- **每段连续2个以上的"的" → 重写该句**
- **"然而/但是/不过/事实上" → 全文不超过3个**
- **结尾在"升华人生道理" → 删掉最后一段试试**
- **所有观点都"合理" → 加一个反直觉或偏执的观点**

**人味制造技法（技术类回答适用）：**
- **偶尔口语化**："就……怎么说呢""反正就那么个意思""说白了就是"
- **偶尔自嘲/吐槽**："我当时也觉得离谱""踩了这个坑之后我悟了"
- **偶尔跑题再绕回来**："扯远了，说回正题""对了这里插一句"
- **不完美的表达**：用"差不多"但不精确的词，句子偶尔写拧了但意思到了
- **偏执和执念**：对某个技术点有不成比例的关注，反复强调
- **留白**：不解释"为什么这样做"，直接给结论。读者自己品。

**终极检测：** 随机抽3段，想象匿名发在V2EX/掘金。会不会有人说"AI写的"？如果有1%的可能，那段必须改。

将文章写入 `workspace/stories/zhihu/YYYY-MM-DD-<slug>.md`（持久化存储，不用 /tmp/）

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
