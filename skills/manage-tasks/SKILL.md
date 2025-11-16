---
name: manage-tasks
description: Manage tasks across hierarchy levels including listing, updating status, prioritizing, and archiving completed tasks.
---

# Task Management Skill

Manage tasks across the hierarchy with listing, status updates, prioritization, and archiving.

## Task Operations

### List Tasks

**All pending tasks**:
```bash
find ~/Projects -path "*/docs/tasks/*.md" -exec grep -l "Status: Pending" {} \;
```

**By level**:
```bash
# Global tasks
cat ~/Projects/docs/tasks/global.md | grep "Status: Pending" -A 10

# Profile tasks
cat ~/Projects/work/docs/tasks/profile.md | grep "Status: Pending" -A 10
```

**By priority**:
```bash
# High priority tasks
grep -r "Priority: High" ~/Projects/*/docs/tasks/ -A 5
```

### Update Task Status

**Mark in progress**:
```bash
# Update status
sed -i '' 's/Status: Pending/Status: In Progress/' task_file

# Add started date
sed -i '' '/Status: In Progress/a\
**Started**: '$(date +%Y-%m-%d)'' task_file
```

**Mark completed**:
```bash
# Update status
sed -i '' 's/Status: In Progress/Status: Completed/' task_file

# Add completed date
sed -i '' '/Status: Completed/a\
**Completed**: '$(date +%Y-%m-%d)'' task_file

# Check all acceptance criteria
sed -i '' 's/- \[ \]/- [x]/' task_file
```

**Mark blocked**:
```bash
# Update status with blocker info
sed -i '' 's/Status: In Progress/Status: Blocked/' task_file

# Add blocker note
echo "**Blocker**: ${blocker_reason}" >> task_file
```

### Prioritize Tasks

**Reprioritize**:
```bash
# Change priority
sed -i '' 's/Priority: Medium/Priority: High/' task_file
```

**Sort by priority**:
```bash
# List tasks sorted by priority
grep -r "##.*-" ~/Projects/*/docs/tasks/ | \
  while read line; do
    priority=$(echo "$line" | grep -o "Priority: [^*]*")
    echo "$priority: $line"
  done | sort
```

### Archive Completed Tasks

**Monthly archive**:
```bash
# Create archive directory
mkdir -p ~/Projects/docs/tasks/archive/2025-11/

# Move completed tasks
# Keep task files clean with only active/blocked tasks
```

**Archive format**:
```markdown
# Completed Tasks - November 2025

## 2025-11-01 - Task Title
**Status**: Completed on 2025-11-05
**Result**: [What was accomplished]

[Original task content]
```

## Task Dashboard

**Generate summary**:
```markdown
# Task Dashboard - 2025-11-11

## Overview
- Total active tasks: 23
- Pending: 15
- In Progress: 5
- Blocked: 3
- Completed this week: 8

## By Priority
- Critical: 2
- High: 7
- Medium: 10
- Low: 4

## By Level
- Global: 2 tasks
- Profile (work): 15 tasks
- Profile (pjbeyer): 4 tasks
- Projects: 2 tasks

## Blocked Tasks
1. [Task 1] - Blocked by: [Reason]
2. [Task 3] - Blocked by: [Reason]

## This Week's Completions
- [Task A] - Completed 2025-11-08
- [Task B] - Completed 2025-11-10
```

## Reporting

### Weekly Summary
Generate report of task progress:
```bash
#!/bin/bash
# Generate weekly task summary

echo "# Weekly Task Summary"
echo "Week of: $(date +%Y-%m-%d)"
echo ""

echo "## Completed"
find ~/Projects -path "*/docs/tasks/*.md" -exec grep -l "Completed.*$(date +%Y-%m-)" {} \;

echo "## In Progress"
find ~/Projects -path "*/docs/tasks/*.md" -exec grep -l "Status: In Progress" {} \;

echo "## Newly Added"
find ~/Projects -path "*/docs/tasks/*.md" -mtime -7
```

### Blocked Task Report
Focus on removing blockers:
```markdown
# Blocked Tasks Report

## High Priority Blocked
1. **[Task Title]**
   - Blocker: [Description]
   - Action: [How to unblock]
   - Owner: [Who can help]

## Actions to Unblock
- [ ] Action 1
- [ ] Action 2
```

## Integration with External Systems

### Jira Sync (work profile)
```bash
# Sync work tasks with Jira
# Update Jira status based on task file
# Pull Jira updates to task file
```

### Calendar Integration
```bash
# Add task deadlines to calendar
# Get reminders for due tasks
```

## Integration

Invoked by:
- Task review workflows
- Weekly planning sessions
- Project status updates
- Blocker identification
