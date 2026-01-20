---
name: workflows:plan
description: Transform feature descriptions into well-structured project plans following conventions
argument-hint: "[feature description, bug report, or improvement idea]"
---

# Create a plan for a new feature or bug fix

## Introduction

Transform feature descriptions, bug reports, or improvement ideas into well-structured markdown plan files that follow project conventions and best practices.

## Feature Description

<feature_description> #$ARGUMENTS </feature_description>

**If the feature description above is empty, ask the user:** "What would you like to plan? Please describe the feature, bug fix, or improvement you have in mind."

Do not proceed until you have a clear feature description from the user.

## Main Tasks

### 1. Repository Research & Context Gathering

Run the research-coordinator agent to gather comprehensive context:

- Task research-coordinator(feature_description)

**Reference Collection:**

- [ ] Document all research findings with specific file paths (e.g., `src/services/example.ts:42`)
- [ ] Include URLs to external documentation and best practices guides
- [ ] Create a reference list of similar issues or PRs (e.g., `#123`, `#456`)
- [ ] Note any team conventions discovered in `CLAUDE.md` or team documentation

### 2. Issue Planning & Structure

**Title & Categorization:**

- [ ] Draft clear, searchable issue title using conventional format (e.g., `feat: Add user authentication`, `fix: Cart total calculation`)
- [ ] Determine issue type: enhancement, bug, refactor
- [ ] Convert title to kebab-case filename: strip prefix colon, lowercase, hyphens for spaces
  - Example: `feat: Add User Authentication` -> `feat-add-user-authentication.md`

**Content Planning:**

- [ ] Choose appropriate detail level based on issue complexity
- [ ] List all necessary sections for the chosen template
- [ ] Gather supporting materials (error logs, screenshots, design mockups)
- [ ] Prepare code examples or reproduction steps if applicable

### 3. Choose Implementation Detail Level

Select how comprehensive you want the issue to be.

#### MINIMAL (Quick Issue)

**Best for:** Simple bugs, small improvements, clear features

```markdown
[Brief problem/feature description]

## Acceptance Criteria

- [ ] Core requirement 1
- [ ] Core requirement 2

## Context

[Any critical information]

## MVP

### example.ts

```typescript
export class Example {
  private name: string;

  constructor() {
    this.name = "example";
  }
}
```

## References

- Related issue: #[issue_number]
- Documentation: [relevant_docs_url]
```

#### MORE (Standard Issue)

**Best for:** Most features, complex bugs, team collaboration

**Includes everything from MINIMAL plus:**
- Detailed background and motivation
- Technical considerations
- Success metrics
- Dependencies and risks
- Basic implementation suggestions

#### A LOT (Comprehensive Issue)

**Best for:** Major features, architectural changes, complex integrations

**Includes everything from MORE plus:**
- Detailed implementation plan with phases
- Alternative approaches considered
- Extensive technical specifications
- Resource requirements
- Future considerations and extensibility
- Risk mitigation strategies
- Documentation requirements

### 4. Issue Creation & Formatting

**Content Formatting:**

- [ ] Use clear, descriptive headings with proper hierarchy
- [ ] Include code examples in triple backticks with language syntax highlighting
- [ ] Add screenshots/mockups if UI-related
- [ ] Use task lists (- [ ]) for trackable items
- [ ] Add collapsible sections for lengthy logs using `<details>` tags

**Cross-Referencing:**

- [ ] Link to related issues/PRs using #number format
- [ ] Reference specific commits with SHA hashes when relevant
- [ ] Link to code using permalink feature
- [ ] Add links to external resources with descriptive text

### 5. Final Review & Submission

**Pre-submission Checklist:**

- [ ] Title is searchable and descriptive
- [ ] All template sections are complete
- [ ] Links and references are working
- [ ] Acceptance criteria are measurable
- [ ] Add names of files in pseudo code examples and todo lists
- [ ] Add an ERD mermaid diagram if applicable for new model changes

## Output Format

**Filename:** Use the kebab-case filename from Step 2 Title & Categorization.

```
plans/<type>-<descriptive-name>.md
```

Examples:
- `plans/feat-user-authentication-flow.md`
- `plans/fix-checkout-race-condition.md`
- `plans/refactor-api-client-extraction.md`

## Post-Generation Options

After writing the plan file, use the **AskUserQuestion tool** to present these options:

**Question:** "Plan ready at `plans/<issue_title>.md`. What would you like to do next?"

**Options:**
1. **Open plan in editor** - Open the plan file for review
2. **Run `/deepen-plan`** - Enhance each section with parallel research agents
3. **Run `/plan_review`** - Get feedback from reviewers (Architecture, Security, Quality)
4. **Start `/workflows:work`** - Begin implementing this plan locally
5. **Create Issue** - Create issue in project tracker (GitHub/Linear)
6. **Simplify** - Reduce detail level

Based on selection:
- **Open plan in editor** -> Run `open plans/<issue_title>.md`
- **`/deepen-plan`** -> Call the /deepen-plan command with the plan file path
- **`/plan_review`** -> Call the /plan_review command with the plan file path
- **`/workflows:work`** -> Call the /workflows:work command with the plan file path
- **Create Issue** -> Use `gh issue create` or linear CLI
- **Simplify** -> Ask "What should I simplify?" then regenerate simpler version

## Issue Creation

When user selects "Create Issue":

1. **If GitHub:**
   ```bash
   gh issue create --title "feat: [Plan Title]" --body-file plans/<issue_title>.md
   ```

2. **If Linear:**
   ```bash
   linear issue create --title "[Plan Title]" --description "$(cat plans/<issue_title>.md)"
   ```

3. **After creation:**
   - Display the issue URL
   - Ask if they want to proceed to `/workflows:work` or `/plan_review`

NEVER CODE! Just research and write the plan.
