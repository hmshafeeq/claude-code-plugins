---
name: workflows:review
description: Perform exhaustive code reviews using multi-agent analysis and worktrees
argument-hint: "[PR number, GitHub URL, branch name, or latest]"
---

# Review Command

Perform exhaustive code reviews using multi-agent analysis and Git worktrees for deep local inspection.

## Prerequisites

- Git repository with GitHub CLI (`gh`) installed and authenticated
- Clean main/master branch
- Proper permissions to create worktrees and access the repository

## Main Tasks

### 1. Determine Review Target & Setup

<review_target> #$ARGUMENTS </review_target>

#### Immediate Actions:

- [ ] Determine review type: PR number (numeric), GitHub URL, file path (.md), or empty (current branch)
- [ ] Check current git branch
- [ ] If ALREADY on the PR branch -> proceed with analysis on current branch
- [ ] If DIFFERENT branch -> offer to use worktree: Call `skill: git-worktree`
- [ ] Fetch PR metadata using `gh pr view --json` for title, body, files, linked issues
- [ ] Make sure we are on the branch we are reviewing

#### Parallel Agents to review the PR:

Run ALL or most of these agents at the same time:

1. Task git-history-analyzer(PR content)
2. Task code-quality-analyst(PR content)
3. Task architecture-strategist(PR content)
4. Task security-sentinel(PR content)
5. Task performance-oracle(PR content)

#### Conditional Agents (Run if applicable):

**If PR contains database migrations or schema changes:**

Run additional research agents to validate data integrity concerns.

### 2. Deep Dive Analysis

For each finding from agents, evaluate:
- What's the actual impact?
- Is this a blocker or nice-to-have?
- What's the fix?

### 3. Findings Synthesis and Todo Creation

**Use file-todos skill** to create todo files for ALL findings immediately.

#### Step 1: Synthesize All Findings

- [ ] Collect findings from all parallel agents
- [ ] Categorize by type: security, performance, architecture, quality
- [ ] Assign severity levels: P1 (CRITICAL), P2 (IMPORTANT), P3 (NICE-TO-HAVE)
- [ ] Remove duplicate or overlapping findings
- [ ] Estimate effort for each finding (Small/Medium/Large)

#### Step 2: Create Todo Files Using file-todos Skill

Use the file-todos skill to create todo files for ALL findings:

```bash
skill: file-todos
```

**File naming convention:**
```
{issue_id}-{status}-{priority}-{description}.md

Examples:
- 001-pending-p1-security-vulnerability.md
- 002-pending-p2-performance-optimization.md
- 003-pending-p3-code-cleanup.md
```

**Priority values:**
- `p1` - Critical (blocks merge, security/data issues)
- `p2` - Important (should fix, architectural/performance)
- `p3` - Nice-to-have (enhancements, cleanup)

#### Step 3: Summary Report

After creating all todo files, present comprehensive summary:

```markdown
## Code Review Complete

**Review Target:** PR #XXXX - [PR Title]
**Branch:** [branch-name]

### Findings Summary:

- **Total Findings:** [X]
- **P1 CRITICAL:** [count] - BLOCKS MERGE
- **P2 IMPORTANT:** [count] - Should Fix
- **P3 NICE-TO-HAVE:** [count] - Enhancements

### Created Todo Files:

**P1 - Critical (BLOCKS MERGE):**
- `001-pending-p1-{finding}.md` - {description}

**P2 - Important:**
- `002-pending-p2-{finding}.md` - {description}

**P3 - Nice-to-Have:**
- `003-pending-p3-{finding}.md` - {description}

### Review Agents Used:
- security-sentinel
- performance-oracle
- architecture-strategist
- code-quality-analyst
- git-history-analyzer

### Next Steps:

1. **Address P1 Findings**: CRITICAL - must be fixed before merge
2. **Triage All Todos**: `ls todos/*-pending-*.md`
3. **Work on Approved Todos**: Fix issues as prioritized
```

### 4. End-to-End Testing (Optional)

After presenting the Summary Report, offer appropriate testing based on project type:

**For Web Projects:**
```markdown
**"Want to run browser tests on the affected pages?"**
1. Yes - run browser tests
2. No - skip
```

### Important: P1 Findings Block Merge

Any **P1 (CRITICAL)** findings must be addressed before merging the PR. Present these prominently and ensure they're resolved before accepting the PR.

## Severity Breakdown

**P1 (Critical - Blocks Merge):**
- Security vulnerabilities
- Data corruption risks
- Breaking changes
- Critical architectural issues

**P2 (Important - Should Fix):**
- Performance issues
- Significant architectural concerns
- Major code quality problems
- Reliability issues

**P3 (Nice-to-Have):**
- Minor improvements
- Code cleanup
- Optimization opportunities
- Documentation updates
