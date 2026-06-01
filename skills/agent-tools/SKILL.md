---
name: agent-tools
description: Scan project tech stack, recommend Claude Code plugins, LSP, MCP, and Skills. Triggered by: "推荐插件", "这个项目需要什么", "配置项目", "setup project", "install plugins", "what does this project need".
---

# Agent Tools Recommender

Scan the project directory, identify the tech stack, and recommend suitable Claude Code plugins, LSP servers, MCP servers, and Skills.

## Workflow

### 1. Identify project tech stack

Do not use a hardcoded file-to-language mapping table. Scan the project directory and use your own judgment:
- Look at directory structure, config files, source code, dependency files
- Read key config files (e.g. package.json, pom.xml, go.mod, etc.)
- Identify language, framework, build tools, package manager, test framework, etc.
- Also identify frontend tech stack if present

### 2. Check currently installed plugins and Skills

Do not assume a fixed management command. Figure out what's actually available in the current environment:
- Read plugin config files to see what's already installed
- Use whatever method the current environment supports to query existing MCP servers
- Check installed skills
- Record everything so you can skip already-installed items in recommendations

### 3. Search and recommend

Do not do mechanical "language X → plugin X" mapping. Actually analyze the project's real needs:

- What is this project **missing**? What's already covered, and what gaps remain?
- What is the project's **workflow**? (e.g., has database → recommend database MCP; has lots of tests → recommend test-related skill; uses Docker → recommend container tools; has API → recommend API debugging tools)
- Recommendations must be **genuinely useful**, not padding. Better to recommend fewer high-quality items than a long list nobody will install

Use WebSearch to confirm recommended items actually exist and are well-maintained. Also check the marketplace directory for available plugins.

For each candidate recommendation, verify quality:
- GitHub repo star count, recent update time, issue activity
- npm/pypi weekly download counts
- Prefer plugins from the official marketplace
- Do not recommend outdated, inactive, or unmaintained projects

### 4. Output recommendation report

Keep it concise. Each recommendation should explain **why it's useful for this specific project** (not "because you use Java", but "your project has heavy MyBatis SQL, and LSP can help with type checking").

Skip already-installed items. If WebSearch returns no results, recommend based on known information — do not fabricate plugins that don't exist.
