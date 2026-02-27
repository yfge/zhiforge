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
三种来源（优先级从高到低）：
- **邀请回答**：`https://www.zhihu.com/creator/featured-question/invited`（转化率最高）
- **搜索热点**：用专业领域关键词搜索 `https://www.zhihu.com/search?type=content&q=...`
- **手动指定**：用户直接给出问题 URL 或标题

筛选标准（问题质量 > 故事质量）：
- 与知识库匹配度高（AI编程/vibe-coding/Claude Code 等）
- **关注者数 > 100** 的优先（关注者越多推荐流量越大）
- **回答数 < 200** 的优先（太多回答难以获得曝光）
- **浏览量 > 10K** 的优先
- 排除已回答过的（标题相似度 > 80%）
- **问题调性匹配**：问题的受众要和文章内容对得上，否则阅读量高但赞同为 0

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

**写完文章后，必须读 `references/anti-ai.md` 逐条过检查。** 该文件是**技术文章专用**的去 AI 味手册（区别于小说版），包含：
- 第一层：句式级 — 通用禁用句式 + 技术文章特有 AI 句式（"X 是一种强大的工具"等）
- 第二层：段落/结构级 — 结构太工整、观点太平衡、代码太教科书
- 第三层：内容级 — 缺具体踩坑细节、没有时间线和情绪、数据太精确
- 第四层：语气级 — 人味制造技法 + 避免讲师腔/官方腔/鸡汤腔
- 终极检测流程（4 步扫描 + V2EX/掘金测试）

**速查禁用句式（完整列表见 anti-ai.md）：**
```
# 通用 AI 八股
❌ "不是A，而是B" / "与其说A，不如说B" / "这不仅仅是A，更是B"
❌ "某种意义上来说" / "在这个A的时代" / "值得一提的是"
❌ "不得不说" / "毫无疑问" / "A，而非B" / "某种程度上"
❌ "事实上"作为段落开头 / "然而"作为段落开头（全文最多2次）
❌ "总的来说" / "综上所述" / "希望这篇文章对你有所帮助"
❌ "首先……其次……最后……"（三段论八股）

# 技术文章特有
❌ "X 是一种强大的工具/框架" / "X 为开发者提供了……"
❌ "在实际开发中，我们通常会……"（伪经验）
❌ "选择 X 还是 Y 取决于具体需求"（万能废话）
❌ "随着 X 的不断发展……" / "X 的核心理念是……"
❌ "总结出一个核心实践/方法论/最佳实践"（培训PPT腔）

# 排版指纹（不是句式，但一样暴露 AI）
❌ 括号注释排比：连续 3 个 "XX（解释），YY（解释），ZZ（解释）"
❌ 加粗金句段过多：全文超过 3 个加粗独句段 = PPT感
❌ "第一步→第二步→第三步" 编号标题：去掉编号，用自然过渡
```

**技术类回答的人味制造技法：**
- **偶尔口语化**："就……怎么说呢""反正就那么个意思""说白了就是"
- **偶尔自嘲/吐槽**："我当时也觉得离谱""踩了这个坑之后我悟了"
- **偶尔跑题再绕回来**："扯远了，说回正题""对了这里插一句"
- **不完美的表达**：用"差不多"但不精确的词，句子偶尔写拧了但意思到了
- **偏执和执念**：对某个技术点有不成比例的关注，反复强调
- **留白**：不解释"为什么这样做"，直接给结论。读者自己品。

**终极检测：** 随机抽3段，想象匿名发在V2EX/掘金。会不会有人说"AI写的"？如果有1%的可能，那段必须改。

将文章写入 `workspace/stories/zhihu/YYYY-MM-DD-<slug>.md`（持久化存储，不用 /tmp/）

### Step 5b: 博客直发模式（从已有博客文章发布）
当用户指定一篇已有的博客文章（而非自动匹配生成）时：
1. 读取博客原文
2. **适当改写**为知乎风格（去掉 Jekyll frontmatter、调整格式、加知乎引导语）
3. 不需要大幅重写——博客本身就是知识库内容
4. 保存到 `workspace/stories/zhihu/YYYY-MM-DD-<slug>.md`
5. 跳到 Step 6 发布

### Step 6: 发布文章
**必须使用专栏页面发布**（避免反爬）：

#### 6a. 准备文件
1. 将 markdown 文件复制到浏览器上传目录：
   ```
   /var/folders/zp/8m3zqqgj2fb0_njgh6scd8qm0000gn/T/openclaw-501/uploads/
   ```
   **必须！** browser upload 工具只能读取这个目录下的文件。

#### 6b. 导入 Markdown
1. 打开 `https://zhuanlan.zhihu.com/write`（每次新页面，导入是追加不是替换）
2. snapshot 获取页面结构
3. 找到并点击 **"导入"** 按钮（通常在工具栏区域，文字为"导入"）
4. 在弹出的下拉/菜单中，点击 **"导入文档"**
5. 此时会触发文件选择对话框 → 使用 `browser action=upload paths=["/path/to/uploads/xxx.md"]`
6. 等待 2-3 秒，正文区域出现导入内容
7. snapshot 确认正文已有内容

#### 6c. 填写标题
1. 找到标题输入框（placeholder 通常为"请输入标题"）
2. click 聚焦标题框
3. type 输入标题文字

#### 6d. 投稿至问题（可选但强烈推荐）
1. 在页面右侧或底部找到"投稿至问题"区域
2. 如果是折叠的，先 click 展开
3. 在搜索框中 type 输入问题关键词（2-4 个字即可，太长搜不到）
4. 等待 1-2 秒出现搜索结果
5. 在结果列表中找到目标问题，点击 **"选择"** 按钮
6. 确认问题已选中（显示问题标题）

#### 6e. 发布
1. 点击 **"发布"** 按钮
2. 如果弹出确认对话框（如话题选择），按需处理后确认
3. 等待页面跳转，URL 变为 `zhuanlan.zhihu.com/p/xxxxx` 表示成功
4. 记录发布链接

**关键坑点：**
- **文件必须在 uploads 目录**：browser upload 无法读取其他路径
- 不能直接 fill 正文，markdown 语法会变纯文本 → 必须用导入
- 导入是追加不是替换 → 每次打开新的 write 页面
- 投稿至问题的搜索框是 combobox → 先 click 聚焦再输入
- 搜索关键词不宜太长，2-4 个核心词效果最好
- 对话框用 Escape 关闭
- 发布按钮可能 disabled → 要等标题和正文都有内容
- **发布后默认开启赞赏**（送礼物设置 → 开启）

### Step 7: 质量审核（交叉验证）
发布后，用另一个模型（如 qwen）审核文章质量：
- 标题吸引力、开头力度、论据充实度
- 逻辑结构、知乎风格匹配、结尾引导
- 总分和改进建议

使用 `sessions_spawn` + 不同 model 参数来实现交叉验证。

### Step 8: 回存知识库（闭环）
如果质量审核通过：
1. 在文章开头加上 Jekyll frontmatter（layout: post、title、date、tags）
2. 保存到 `blog_repo/_posts/YYYY-MM-DD-标题.md`
3. `git add` + `git commit` + `git push origin master`
   - **注意**：blog repo 的默认分支是 `master`（不是 main）

这样知乎文章反向充实了博客知识库，形成正向循环。

**博客直发模式下跳过此步**（文章本来就来自博客）。

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
| 启动浏览器 | `browser action=start profile="openclaw"` |
| 打开页面 | `browser action=navigate targetUrl="..."` |
| 获取内容 | `browser action=snapshot compact=true` |
| 点击 | `browser action=act request={kind:"click", ref:"..."}` |
| 输入 | `browser action=act request={kind:"type", ref:"...", text:"..."}` |
| 按键 | `browser action=act request={kind:"press", key:"Escape"}` |
| 上传文件 | `browser action=upload paths=["/path/to/uploads/file"]` |
| 截图 | `browser action=screenshot` |
| 等待 | `browser action=act request={kind:"wait", timeMs:3000}` |
| 关闭浏览器 | `browser action=stop` |

**重要规则：**
- 文件上传路径必须是 uploads 目录：`/var/folders/zp/8m3zqqgj2fb0_njgh6scd8qm0000gn/T/openclaw-501/uploads/`
- 先 `cp` 文件到 uploads 目录，再用 `browser action=upload`
- **用完浏览器后必须 `browser action=stop`**，不要让 Chrome 一直开着

## 其他命令

- **"检查知乎邀请"**：只执行 Step 2-3，列出可回答问题
- **"草拟知乎回答 [问题]"**：只执行 Step 4-5，不发布
- **"知乎热点"**：搜索热点 + 全流程
- **"发博客到知乎 [文件名/路径]"**：博客直发模式（Step 5b → 6 → 7），跳过知识库匹配
- **"发到知乎 [问题URL]"**：指定问题投稿，跳过 Step 3 搜索

详细命令模板见 `{baseDir}/.claude/commands/*.md`

## 注意事项

- **全程自动执行**，遇到问题自行决策或跳过
- **知识库匹配度低的问题要跳过**，不要强行回答
- 保持作者风格：务实、不油腻、有干货
- 发布失败重试一次，仍失败则输出错误信息
- 连续 3 个问题都不合适则结束
