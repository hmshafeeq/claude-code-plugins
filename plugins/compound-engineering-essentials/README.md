# Compound Engineering Essentials

Framework-agnostic AI-powered development tools that make each unit of engineering work easier than the last.

**Repository:** [github.com/hmshafeeq/claude-code-plugins](https://github.com/hmshafeeq/claude-code-plugins)

## Components

| Component | Count |
|-----------|-------|
| Agents | 8 |
| Commands | 13 |
| Skills | 6 |

## Agents

### Review (4)

| Agent | Description |
|-------|-------------|
| `code-quality-analyst` | Analyze code for patterns, anti-patterns, simplicity, and YAGNI violations |
| `architecture-strategist` | Analyze architectural decisions and compliance |
| `security-sentinel` | Security audits and vulnerability assessments |
| `performance-oracle` | Performance analysis and optimization |

### Research (2)

| Agent | Description |
|-------|-------------|
| `research-coordinator` | Coordinate research across skills, docs, repos, and external sources |
| `git-history-analyzer` | Analyze git history and code evolution |

### Workflow (2)

| Agent | Description |
|-------|-------------|
| `pr-comment-resolver` | Address PR comments and implement fixes |
| `bug-reproduction-validator` | Systematically reproduce and validate bug reports |

## Commands

### Workflow Commands

Core workflow commands use `workflows:` prefix to avoid collisions with built-in commands:

| Command | Description |
|---------|-------------|
| `/workflows:plan` | Create implementation plans |
| `/workflows:review` | Run comprehensive code reviews |
| `/workflows:work` | Execute work items systematically |
| `/workflows:compound` | Document solved problems to compound team knowledge |

### Utility Commands

| Command | Description |
|---------|-------------|
| `/deepen-plan` | Enhance plans with parallel research agents for each section |
| `/plan-review` | Multi-agent plan review in parallel |
| `/triage` | Triage and categorize findings for the CLI todo system |
| `/resolve-todo-parallel` | Resolve all pending CLI todos using parallel processing |
| `/utils:generate-command` | Create new custom slash commands following conventions |
| `/utils:changelog` | Create engaging changelogs for recent merges to main branch |
| `/utils:create-agent-skill` | Create or edit Claude Code skills with expert guidance |
| `/utils:test-browser` | Run browser tests on pages affected by current PR or branch |
| `/utils:feature-video` | Record a video walkthrough of a feature and add it to the PR |

## Skills

| Skill | Description |
|-------|-------------|
| `compound-docs` | Document solved problems with YAML frontmatter for searchability |
| `create-agent-skills` | Comprehensive guide for creating Claude Code agents and skills |
| `skill-creator` | Guide for creating effective Claude Code skills |
| `git-worktree` | Manage Git worktrees for parallel development |
| `file-todos` | File-based todo tracking system |
| `agent-browser` | CLI-based browser automation using Vercel's agent-browser |

## Philosophy: Compounding Engineering

**Each unit of engineering work should make subsequent units of work easier—not harder.**

This plugin embodies that philosophy by providing tools that:
- Document solutions as they're created
- Review code comprehensively using multiple perspectives
- Research best practices before implementation
- Track progress systematically

## Core Workflow

```
Plan -> Work -> Review -> Compound -> Repeat
```

1. `/workflows:plan` produces `plans/*.md` - input for `/workflows:work`
2. `/workflows:work` produces PR - input for `/workflows:review`
3. `/workflows:review` produces findings - merge - `/workflows:compound`
4. `/workflows:compound` produces `docs/solutions/*.md` - informs future `/workflows:plan`

## Installation

```bash
# Add marketplace
claude /plugin marketplace add https://github.com/hmshafeeq/claude-code-plugins

# Install plugin
claude /plugin install compound-engineering-essentials
```

### Reinstall / Update

```bash
claude /plugin marketplace remove claude-shafs-marketplace && claude /plugin uninstall compound-engineering-essentials && claude /plugin marketplace add https://github.com/hmshafeeq/claude-code-plugins && claude /plugin install compound-engineering-essentials
```

## License

MIT
