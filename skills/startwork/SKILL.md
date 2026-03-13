---
name: startwork
description: Use when starting a new work session or initializing a new project — sets up CLAUDE.md and progress.md if missing, reads existing progress, and briefs the user on current status
---

# Start Work Session

## What This Does

Initialize a project for session-based work tracking, or resume from where the last session left off.

## Workflow

```dot
digraph startwork {
    "Run /startwork" [shape=doublecircle];
    "CLAUDE.md exists?" [shape=diamond];
    "Create project CLAUDE.md" [shape=box];
    "progress.md exists?" [shape=diamond];
    "Create empty progress.md" [shape=box];
    "Read progress.md" [shape=box];
    "Check git log (if git repo)" [shape=box];
    "Brief user on status" [shape=doublecircle];

    "Run /startwork" -> "CLAUDE.md exists?";
    "CLAUDE.md exists?" -> "progress.md exists?" [label="yes"];
    "CLAUDE.md exists?" -> "Create project CLAUDE.md" [label="no"];
    "Create project CLAUDE.md" -> "progress.md exists?";
    "progress.md exists?" -> "Read progress.md" [label="yes"];
    "progress.md exists?" -> "Create empty progress.md" [label="no"];
    "Create empty progress.md" -> "Brief user on status";
    "Read progress.md" -> "Check git log (if git repo)";
    "Check git log (if git repo)" -> "Brief user on status";
}
```

## Step-by-step

### 1. Project CLAUDE.md

If the project root has no `CLAUDE.md`, create one with this minimal template:

```markdown
# Project Name

## Overview
(brief project description)

## Session Continuity
- At session start, always check `progress.md` for current work status.
- Before ending a session, run `/stopwork` to save progress.
```

If `CLAUDE.md` already exists, leave it as-is.

### 2. progress.md

If `progress.md` does not exist, create it:

```markdown
# Work Progress

## Current Task
- (none yet)

## Last Session
- (first session)

## Next Steps
- (to be determined)

## Key Decisions
- (none yet)
```

If it exists, read it in full.

### 3. Git History (if applicable)

If the project is a git repo, check recent commits since the last session date noted in progress.md:

```bash
git log --oneline -10
```

### 4. Brief the User

Provide a concise summary:
- What was done in the last session
- What the next steps are
- Any recent git changes not captured in progress.md
- Ask the user what they want to work on today
