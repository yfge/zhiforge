# 查看知乎邀请并匹配博客

## 任务
1. 使用 Chrome MCP 打开知乎 (https://www.zhihu.com)
2. 查看"邀请回答"页面 (https://www.zhihu.com/creator/featured-question/invited)
3. 获取所有邀请问题列表
4. 读取博客目录 `/Users/geyunfei/dev/yfge/yfge.github.com/_posts/` 下的文章
5. 根据 `claude.md` 中的专业领域和博客文章索引，匹配适合回答的问题

## 输出格式
按匹配度分类输出：

### 高度匹配（强烈推荐）
| 问题 | 匹配原因 | 可引用的博客文章 |

### 较匹配（可以回答）
| 问题 | 匹配原因 |

### 不匹配（跳过）
- 列出不匹配的问题

## 注意事项
- 优先推荐与 AI 编程、vibe-coding、Claude Code、Docker、工程化实践相关的问题
- 如果有可以直接引用的博客文章，标注文件名
- 如果用户未登录知乎，提示需要先手动登录
