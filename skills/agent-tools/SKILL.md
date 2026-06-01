---
name: agent-tools
description: 扫描当前项目技术栈，推荐 Claude Code 插件、LSP、MCP 和 Skill。
---

# Agent 工具推荐

扫描项目目录，自动识别技术栈，推荐适合的 Claude Code 插件、LSP、MCP 服务器和 Skill。

## 执行流程

### 1. 识别项目技术栈

不要用固定的文件映射表。直接扫描项目目录，根据你的判断识别：
- 看目录结构、配置文件、源码文件、依赖文件
- 读取关键配置文件的内容（如 package.json、pom.xml、go.mod 等）
- 识别语言、框架、构建工具、包管理器、测试框架等
- 如果有前端代码，也识别前端技术栈

### 2. 查询当前已安装的插件和 Skill

扫描以下配置文件，确保不遗漏：

| 内容 | 路径 |
|---|---|
| 已安装插件 | `~/.claude/plugins/installed_plugins.json` |
| 插件启用状态 | `~/.claude/settings.json` → `enabledPlugins` |
| MCP 服务器（local/user） | `~/.claude.json` → `mcpServers` 和 `projects.<路径>.mcpServers` |
| MCP 服务器（project） | `./.mcp.json` → `mcpServers` |
| 插件自带 MCP | `~/.claude/plugins/cache/*/` 下的 `.mcp.json` |
| Skills（用户级） | `~/.claude/skills/` |
| Skills（项目级） | `./.claude/skills/` 或 `./.claude/commands/` |
| Hooks | `~/.claude/settings.json` 和 `./.claude/settings.json` 和 `./.claude/settings.local.json` → `hooks` |
| Rules | `~/.claude/rules/` 和 `./.claude/rules/` |

**注意**：`~/.claude.json` 和 `~/.claude/settings.json` 是两个不同的文件！前者存储 MCP 服务器和运行时状态，后者存储行为配置（权限、hooks、环境变量等）。`claude mcp add` 默认写入 `~/.claude.json`。

### 3. 搜索推荐

不要只按"用了什么语言就推什么插件"这种机械映射。要真正分析项目的实际需求：

- 这个项目**缺什么**？已有的工具覆盖了什么，还差什么？
- 项目的**工作流**是什么样的？（比如：有数据库 → 推数据库 MCP；有大量测试 → 推测试相关 skill；用 Docker → 推容器相关工具；有 API → 推 API 调试工具）
- 推荐的东西要**真的有用**，不是凑数。宁可少推荐几个高质量的，也不要列一堆用户装了也不会用的

用 WebSearch 查询确认推荐的东西确实存在且好用。同时检查 marketplace 目录中已有的可用插件。

对每个候选推荐，必须验证质量：
- GitHub 仓库的 star 数、最近更新时间、issue 活跃度
- npm/pypi 等包管理器的周下载量
- 如果是官方 marketplace 里的插件，优先推荐
- 过时、不活跃、无人维护的不要推荐

### 4. 输出推荐报告

简洁明了，每个推荐说清楚**为什么推荐这个**（不是"因为你用了 Java"，而是"你项目里有大量 MyBatis SQL，LSP 能帮你做类型检查"）。

已安装的标记跳过。如果 WebSearch 没找到结果，基于已知信息推荐，不要编造不存在的插件。
