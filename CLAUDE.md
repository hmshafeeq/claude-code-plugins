# Claude Code Plugins - Plugin Marketplace

A Claude Code plugin marketplace featuring framework-agnostic development tools.

**Available Plugin:**
- `compound-engineering-essentials` - Framework-agnostic AI-powered development tools for any tech stack

## Repository Structure

```
claude-code-plugins/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace catalog
└── plugins/
    └── compound-engineering-essentials/
        ├── .claude-plugin/
        │   └── plugin.json       # Plugin metadata
        ├── agents/               # 8 specialized AI agents
        ├── commands/             # 13 slash commands
        ├── skills/               # 6 skills
        └── README.md             # Plugin documentation
```

## Philosophy: Compounding Engineering

**Each unit of engineering work should make subsequent units of work easier—not harder.**

When working on this repository, follow the compounding engineering process:

1. **Plan** → Understand the change needed and its impact
2. **Delegate** → Use AI tools to help with implementation
3. **Assess** → Verify changes work as expected
4. **Codify** → Update this CLAUDE.md with learnings

## Working with This Repository

### Updating the Plugin

When agents, commands, or skills are added/removed, follow this checklist:

#### 1. Count all components accurately

```bash
# Count agents
find plugins/compound-engineering-essentials/agents -name "*.md" | wc -l

# Count commands
find plugins/compound-engineering-essentials/commands -name "*.md" | wc -l

# Count skills
ls -d plugins/compound-engineering-essentials/skills/*/ 2>/dev/null | wc -l
```

#### 2. Update ALL description strings with correct counts

The description appears in multiple places and must match everywhere:

- [ ] `plugins/compound-engineering-essentials/.claude-plugin/plugin.json` → `description` field
- [ ] `.claude-plugin/marketplace.json` → plugin `description` field
- [ ] `plugins/compound-engineering-essentials/README.md` → intro paragraph

Format: `"Includes X specialized agents, Y commands, and Z skill(s)."`

#### 3. Update version numbers

When adding new functionality, bump the version in:

- [ ] `plugins/compound-engineering-essentials/.claude-plugin/plugin.json` → `version`
- [ ] `.claude-plugin/marketplace.json` → plugin `version`

#### 4. Update documentation

- [ ] `plugins/compound-engineering-essentials/README.md` → list all components
- [ ] `CLAUDE.md` → update structure diagram if needed

#### 5. Validate JSON files

```bash
cat .claude-plugin/marketplace.json | jq .
cat plugins/compound-engineering-essentials/.claude-plugin/plugin.json | jq .
```

#### 6. Verify before committing

```bash
# Ensure counts in descriptions match actual files
grep -o "Includes [0-9]* specialized agents" plugins/compound-engineering-essentials/.claude-plugin/plugin.json
find plugins/compound-engineering-essentials/agents -name "*.md" | wc -l
```

### Marketplace.json Structure

The marketplace.json follows the official Claude Code spec:

```json
{
  "name": "marketplace-identifier",
  "owner": {
    "name": "Owner Name",
    "url": "https://github.com/owner"
  },
  "metadata": {
    "description": "Marketplace description",
    "version": "1.0.0"
  },
  "plugins": [
    {
      "name": "plugin-name",
      "description": "Plugin description",
      "version": "1.0.0",
      "author": { ... },
      "homepage": "https://...",
      "tags": ["tag1", "tag2"],
      "source": "./plugins/plugin-name"
    }
  ]
}
```

**Only include fields that are in the official spec.** Do not add custom fields like:

- `downloads`, `stars`, `rating` (display-only)
- `categories`, `featured_plugins`, `trending` (not in spec)
- `type`, `verified`, `featured` (not in spec)

## Testing Changes

### Test Locally

1. Install the marketplace locally:

   ```bash
   claude /plugin marketplace add /path/to/claude-code-plugins
   ```

2. Install the plugin:

   ```bash
   claude /plugin install compound-engineering-essentials
   ```

3. Test agents and commands:
   ```bash
   claude /workflows:review
   claude agent code-quality-analyst "test message"
   ```

### Validate JSON

Before committing, ensure JSON files are valid:

```bash
cat .claude-plugin/marketplace.json | jq .
cat plugins/compound-engineering-essentials/.claude-plugin/plugin.json | jq .
```

## Common Tasks

### Adding a New Agent

1. Create `plugins/compound-engineering-essentials/agents/<category>/new-agent.md`
2. Update plugin.json agent count and agent list
3. Update README.md agent list
4. Test with `claude agent new-agent "test"`

### Adding a New Command

1. Create `plugins/compound-engineering-essentials/commands/new-command.md`
2. Update plugin.json command count and command list
3. Update README.md command list
4. Test with `claude /new-command`

### Adding a New Skill

1. Create skill directory: `plugins/compound-engineering-essentials/skills/skill-name/`
2. Add skill structure:
   ```
   skills/skill-name/
   ├── SKILL.md           # Skill definition with frontmatter (name, description)
   └── scripts/           # Supporting scripts (optional)
   ```
3. Update plugin.json description with new skill count
4. Update marketplace.json description with new skill count
5. Update README.md with skill documentation
6. Test with `claude skill skill-name`

### Updating Tags/Keywords

Tags should reflect the compounding engineering philosophy:

- Use: `ai-powered`, `compound-engineering`, `workflow-automation`, `knowledge-management`, `framework-agnostic`

## Commit Conventions

Follow standard commit message format:

- `feat:` - Adding new functionality
- `fix:` - Bug fixes
- `docs:` - Documentation updates
- `refactor:` - Code changes without new features

## Resources

- [Claude Code Plugin Documentation](https://docs.claude.com/en/docs/claude-code/plugins)
- [Plugin Marketplace Documentation](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces)
- [Plugin Reference](https://docs.claude.com/en/docs/claude-code/plugins-reference)
