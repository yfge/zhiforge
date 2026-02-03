# 贡献指南

感谢你考虑为 ZhiForge 做贡献！

## 如何贡献

### 报告 Bug

如果你发现了 bug，请通过 [GitHub Issues](https://github.com/yfge/zhiforge/issues) 提交报告：

1. 使用 Bug Report 模板
2. 提供清晰的复现步骤
3. 说明期望行为和实际行为
4. 附上环境信息（OS、Claude Code 版本等）

### 提出新功能

如果你有新功能的想法：

1. 先检查 [Issues](https://github.com/yfge/zhiforge/issues) 是否已有类似建议
2. 使用 Feature Request 模板创建新 issue
3. 详细描述使用场景和预期效果

### 提交代码

#### 开发流程

1. **Fork 仓库**
   ```bash
   # 在 GitHub 上 Fork 本仓库
   git clone https://github.com/YOUR_USERNAME/zhiforge.git
   cd zhiforge
   ```

2. **创建分支**
   ```bash
   git checkout -b feature/your-feature-name
   # 或
   git checkout -b fix/your-bug-fix
   ```

3. **进行开发**
   - 遵循项目现有的代码风格
   - 添加必要的注释
   - 确保配置文件格式正确

4. **测试你的改动**
   ```bash
   # 在 Claude Code 中测试相关 skill
   /auto-zhihu  # 或其他相关命令
   ```

5. **提交改动**
   ```bash
   git add .
   git commit -m "feat: 添加 xxx 功能"
   # 或
   git commit -m "fix: 修复 xxx 问题"
   ```

   提交信息格式：
   - `feat:` 新功能
   - `fix:` Bug 修复
   - `docs:` 文档更新
   - `refactor:` 代码重构
   - `test:` 测试相关
   - `chore:` 构建/工具相关

6. **推送到你的 Fork**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **创建 Pull Request**
   - 在 GitHub 上创建 PR
   - 填写 PR 模板
   - 关联相关的 Issue

#### 代码规范

- **配置文件**：使用标准 JSON 格式，2 空格缩进
- **Markdown**：遵循 CommonMark 规范
- **提示词**：清晰、具体，避免歧义
- **注释**：关键逻辑需要中文注释说明

#### PR 审查

- 维护者会在 48 小时内响应
- 可能需要修改代码或补充说明
- 所有 PR 需要至少一位维护者批准

### 改进文档

文档改进同样重要：

- 修正错别字
- 改进说明清晰度
- 添加使用示例
- 翻译文档（如有需要）

## 开发环境设置

### 前置要求
- Claude Code CLI
- Node.js 18+
- Chrome 浏览器
- 知乎账号

### 快速开始
```bash
# 克隆你 Fork 的仓库
git clone https://github.com/YOUR_USERNAME/zhiforge.git
cd zhiforge

# 配置 settings.json
cp .claude/settings.example.json .claude/settings.json
# 编辑 .claude/settings.json

# 启动 Chrome 调试
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222

# 在 Claude Code 中测试
/auto-zhihu
```

## 社区准则

- **尊重他人**：友善、包容、专业
- **建设性反馈**：提出问题时给出建议
- **开放心态**：接受不同观点和方案
- **保护隐私**：不要泄露个人敏感信息

## 需要帮助？

- 查看 [README](README.md) 了解基本用法
- 搜索 [已有 Issues](https://github.com/yfge/zhiforge/issues)
- 提出新的 Issue 寻求帮助

## 许可证

贡献的代码将采用 [MIT License](LICENSE)。

---

再次感谢你的贡献！🎉
