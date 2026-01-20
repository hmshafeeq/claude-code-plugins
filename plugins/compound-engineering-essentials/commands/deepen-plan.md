---
name: deepen-plan
description: Enhance a plan with parallel research agents for each section to add depth, best practices, and implementation details
argument-hint: "[path to plan file]"
---

# Deepen Plan - Power Enhancement Mode

## Introduction

This command takes an existing plan (from `/workflows:plan`) and enhances each section with parallel research agents. Each major element gets its own dedicated research sub-agent to find:
- Best practices and industry patterns
- Performance optimizations
- Quality enhancements and edge cases
- Real-world implementation examples

The result is a deeply grounded, production-ready plan with concrete implementation details.

## Plan File

<plan_path> #$ARGUMENTS </plan_path>

**If the plan path above is empty:**
1. Check for recent plans: `ls -la plans/`
2. Ask the user: "Which plan would you like to deepen? Please provide the path."

Do not proceed until you have a valid plan file path.

## Main Tasks

### 1. Parse and Analyze Plan Structure

**Read the plan file and extract:**
- [ ] Overview/Problem Statement
- [ ] Proposed Solution sections
- [ ] Technical Approach/Architecture
- [ ] Implementation phases/steps
- [ ] Code examples and file references
- [ ] Acceptance criteria
- [ ] Technologies/frameworks mentioned
- [ ] Domain areas (data models, APIs, UI, security, performance)

**Create a section manifest:**
```
Section 1: [Title] - [Brief description of what to research]
Section 2: [Title] - [Brief description of what to research]
...
```

### 2. Discover and Apply Available Skills

**Step 1: Discover ALL available skills from ALL sources**

```bash
# 1. Project-local skills (highest priority)
ls .claude/skills/

# 2. User's global skills
ls ~/.claude/skills/

# 3. Installed plugin skills
find ~/.claude/plugins/cache -type d -name "skills" 2>/dev/null
```

**Step 2: For each discovered skill, read its SKILL.md to understand what it does**

**Step 3: Match skills to plan content**

For each skill discovered:
- Read its SKILL.md description
- Check if any plan sections match the skill's domain
- If there's a match, spawn a sub-agent to apply that skill's knowledge

**Step 4: Spawn a sub-agent for EVERY matched skill**

For each matched skill:
```
Task general-purpose: "You have the [skill-name] skill available at [skill-path].
Read the skill: cat [skill-path]/SKILL.md
Follow the skill's instructions and apply to:
[relevant plan section]
Return the skill's full output"
```

**Spawn ALL skill sub-agents in PARALLEL.**

### 3. Discover and Apply Learnings/Solutions

Check for documented learnings from /workflows:compound:

```bash
# Find ALL learning markdown files
find docs/solutions -name "*.md" -type f 2>/dev/null
```

**For each learning file, quickly scan its frontmatter** to filter for relevance:

- `tags:` - Do any tags match technologies/patterns in the plan?
- `category:` - Is this category relevant?
- `module:` - Does the plan touch this module?

**Spawn sub-agents for filtered learnings:**

```
Task general-purpose: "
LEARNING FILE: [full path to .md file]

Read this learning file and check if it applies to this plan:
[full plan content]

If relevant:
- Explain specifically how it applies
- Quote the key insight or solution
- Suggest where/how to incorporate it

If NOT relevant: Say 'Not applicable: [reason]'
"
```

### 4. Launch Per-Section Research Agents

For each major section, launch parallel research:

```
Task Explore: "Research best practices, patterns, and real-world examples for: [section topic].
Find:
- Industry standards and conventions
- Performance considerations
- Common pitfalls and how to avoid them
Return concrete, actionable recommendations."
```

### 5. Run ALL Review Agents

Discover every available agent and run them against the plan:

```bash
# Find all agents
find .claude/agents -name "*.md" 2>/dev/null
find ~/.claude/agents -name "*.md" 2>/dev/null
find ~/.claude/plugins/cache -path "*/agents/*.md" 2>/dev/null
```

**For compound-engineering-essentials agents specifically:**
- USE: `agents/review/*` (all reviewers)
- USE: `agents/research/*` (all researchers)
- SKIP: `agents/workflow/*` (these are workflow orchestrators)

**Launch ALL agents in parallel:**

```
Task [agent-name]: "Review this plan using your expertise. Apply all your checks and patterns. Plan content: [full plan content]"
```

### 6. Synthesize Everything

**Collect outputs from ALL sources:**

1. **Skill-based sub-agents** - Patterns, code examples, recommendations
2. **Learnings/Solutions sub-agents** - Relevant documented solutions
3. **Research agents** - Best practices, documentation, examples
4. **Review agents** - Feedback from architecture, security, performance, quality

**For each agent's findings, extract:**
- [ ] Concrete recommendations (actionable items)
- [ ] Code patterns and examples
- [ ] Anti-patterns to avoid (warnings)
- [ ] Performance considerations
- [ ] Security considerations
- [ ] Edge cases discovered

**Deduplicate and prioritize:**
- Merge similar recommendations from multiple agents
- Prioritize by impact (high-value improvements first)
- Flag conflicting advice for human review
- Group by plan section

### 7. Enhance Plan Sections

**Enhancement format for each section:**

```markdown
## [Original Section Title]

[Original content preserved]

### Research Insights

**Best Practices:**
- [Concrete recommendation 1]
- [Concrete recommendation 2]

**Performance Considerations:**
- [Optimization opportunity]
- [Benchmark or metric to target]

**Implementation Details:**
```[language]
// Concrete code example from research
```

**Edge Cases:**
- [Edge case 1 and how to handle]
- [Edge case 2 and how to handle]

**References:**
- [Documentation URL 1]
- [Documentation URL 2]
```

### 8. Add Enhancement Summary

At the top of the plan, add a summary section:

```markdown
## Enhancement Summary

**Deepened on:** [Date]
**Sections enhanced:** [Count]
**Research agents used:** [List]

### Key Improvements
1. [Major improvement 1]
2. [Major improvement 2]
3. [Major improvement 3]

### New Considerations Discovered
- [Important finding 1]
- [Important finding 2]
```

### 9. Update Plan File

**Write the enhanced plan:**
- Preserve original filename
- Add `-deepened` suffix if user prefers a new file
- Update any timestamps or metadata

## Quality Checks

Before finalizing:
- [ ] All original content preserved
- [ ] Research insights clearly marked and attributed
- [ ] Code examples are syntactically correct
- [ ] Links are valid and relevant
- [ ] No contradictions between sections
- [ ] Enhancement summary accurately reflects changes

## Post-Enhancement Options

After writing the enhanced plan, use the **AskUserQuestion tool** to present these options:

**Question:** "Plan deepened at `[plan_path]`. What would you like to do next?"

**Options:**
1. **View diff** - Show what was added/changed
2. **Run `/plan_review`** - Get feedback from reviewers on enhanced plan
3. **Start `/workflows:work`** - Begin implementing this enhanced plan
4. **Deepen further** - Run another round of research on specific sections
5. **Revert** - Restore original plan

NEVER CODE! Just research and enhance the plan.
