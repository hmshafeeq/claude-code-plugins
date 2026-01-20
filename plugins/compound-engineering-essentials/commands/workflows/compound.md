---
name: workflows:compound
description: Document a recently solved problem to compound your team's knowledge
argument-hint: "[optional: brief context about the fix]"
---

# /compound

Coordinate multiple subagents working in parallel to document a recently solved problem.

## Purpose

Captures problem solutions while context is fresh, creating structured documentation in `docs/solutions/` with YAML frontmatter for searchability and future reference. Uses parallel subagents for maximum efficiency.

**Why "compound"?** Each documented solution compounds your team's knowledge. The first time you solve a problem takes research. Document it, and the next occurrence takes minutes. Knowledge compounds.

## Usage

```bash
/workflows:compound                    # Document the most recent fix
/workflows:compound [brief context]    # Provide additional context hint
```

## Execution Strategy: Parallel Subagents

This command launches multiple specialized subagents IN PARALLEL to maximize efficiency:

### 1. **Context Analyzer** (Parallel)
   - Extracts conversation history
   - Identifies problem type, component, symptoms
   - Validates against schema
   - Returns: YAML frontmatter skeleton

### 2. **Solution Extractor** (Parallel)
   - Analyzes all investigation steps
   - Identifies root cause
   - Extracts working solution with code examples
   - Returns: Solution content block

### 3. **Related Docs Finder** (Parallel)
   - Searches `docs/solutions/` for related documentation
   - Identifies cross-references and links
   - Finds related GitHub issues
   - Returns: Links and relationships

### 4. **Prevention Strategist** (Parallel)
   - Develops prevention strategies
   - Creates best practices guidance
   - Generates test cases if applicable
   - Returns: Prevention/testing content

### 5. **Category Classifier** (Parallel)
   - Determines optimal `docs/solutions/` category
   - Validates category against schema
   - Suggests filename based on slug
   - Returns: Final path and filename

### 6. **Documentation Writer** (Parallel)
   - Assembles complete markdown file
   - Validates YAML frontmatter
   - Formats content for readability
   - Creates the file in correct location

### 7. **Optional: Specialized Agent Invocation** (Post-Documentation)
   Based on problem type detected, automatically invoke applicable agents:
   - **performance_issue** -> `performance-oracle`
   - **security_issue** -> `security-sentinel`
   - Any code-heavy issue -> `code-quality-analyst`

## What It Captures

- **Problem symptom**: Exact error messages, observable behavior
- **Investigation steps tried**: What didn't work and why
- **Root cause analysis**: Technical explanation
- **Working solution**: Step-by-step fix with code examples
- **Prevention strategies**: How to avoid in future
- **Cross-references**: Links to related issues and docs

## Preconditions

- Problem has been solved (not in-progress)
- Solution has been verified working
- Non-trivial problem (not simple typo or obvious error)

## What It Creates

**Organized documentation:**

- File: `docs/solutions/[category]/[filename].md`

**Categories auto-detected from problem:**

- build-errors/
- test-failures/
- runtime-errors/
- performance-issues/
- database-issues/
- security-issues/
- ui-bugs/
- integration-issues/
- logic-errors/

## Success Output

```
Parallel documentation generation complete

Primary Subagent Results:
  Context Analyzer: Identified performance_issue in user_service
  Solution Extractor: Extracted 3 code fixes
  Related Docs Finder: Found 2 related issues
  Prevention Strategist: Generated test cases
  Category Classifier: docs/solutions/performance-issues/
  Documentation Writer: Created complete markdown

Specialized Agent Reviews (Auto-Triggered):
  performance-oracle: Validated query optimization approach
  code-quality-analyst: Solution is appropriately minimal

File created:
- docs/solutions/performance-issues/n-plus-one-user-list.md

This documentation will be searchable for future reference when similar
issues occur.

What's next?
1. Continue workflow (recommended)
2. Link related documentation
3. Update other references
4. View documentation
5. Other
```

## The Compounding Philosophy

This creates a compounding knowledge system:

1. First time you solve "N+1 query" -> Research (30 min)
2. Document the solution -> docs/solutions/performance-issues/n-plus-one.md (5 min)
3. Next time similar issue occurs -> Quick lookup (2 min)
4. Knowledge compounds -> Team gets smarter

**Each unit of engineering work should make subsequent units of work easier—not harder.**

## Auto-Invoke

**Trigger phrases:**
- "that worked"
- "it's fixed"
- "working now"
- "problem solved"

**Manual override:** Use /workflows:compound [context] to document immediately without waiting for auto-detection.

## Routes To

`compound-docs` skill

## Applicable Specialized Agents

Based on problem type, these agents can enhance documentation:

### Code Quality & Review
- **code-quality-analyst**: Ensures solution code is minimal and follows patterns
- **architecture-strategist**: Reviews architectural decisions

### Specific Domain Experts
- **performance-oracle**: Analyzes performance_issue category solutions
- **security-sentinel**: Reviews security_issue solutions for vulnerabilities

### Enhancement & Research
- **research-coordinator**: Enriches solution with best practices and documentation

### When to Invoke
- **Auto-triggered** (optional): Agents can run post-documentation for enhancement
- **Manual trigger**: User can invoke agents after /workflows:compound completes for deeper review

## Related Commands

- `/workflows:plan` - Planning workflow (references documented solutions)
- `/workflows:review` - Code review (uses similar agents)
