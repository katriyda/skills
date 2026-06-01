# agent-tools

Claude Code 插件：扫描项目技术栈，智能推荐适合的插件、LSP、MCP 服务器和 Skill。

## 功能

- 自动识别项目语言、框架、工具链
- 查询已安装的插件，避免重复推荐
- WebSearch 验证推荐质量（活跃度、维护状态）
- 输出简洁的推荐报告 + 安装命令

## 安装

```bash
claude plugins add <your-github-username>/agent-tools
```

## 使用

在 Claude Code 中说：

- "推荐插件"
- "这个项目需要什么"
- "配置项目"
- "setup project"
