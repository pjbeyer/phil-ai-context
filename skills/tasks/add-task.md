---
name: add-task
description: Add task to hierarchy-aware task management system with proper context and level placement.
---

# Add Task Skill

Add tasks to the appropriate hierarchy level with proper context awareness.

## Task Hierarchy

### Global Tasks
**Location**: `~/Projects/docs/tasks/global.md`
**Scope**: Cross-profile tasks, environment setup, global improvements

### Profile Tasks
**Location**: `{profilePath}/docs/tasks/profile.md`
**Scope**: Profile-specific tasks, standards updates, profile optimization

### Project Tasks
**Location**: `{projectPath}/docs/tasks/project.md`
**Scope**: Project-specific tasks, features, fixes, improvements

### Agent Tasks
**Location**: `{agentPath}/docs/tasks/agent.md`
**Scope**: Agent-specific improvements, capability additions

## Task Format

```markdown
## [Date] - [Task Title]

**Priority**: [Critical|High|Medium|Low]
**Status**: [Pending|In Progress|Completed|Blocked]
**Assigned**: [Person or Auto]
**Context**: [Brief context]

**Task**:
[What needs to be done]

**Acceptance Criteria**:
- [ ] Criterion 1
- [ ] Criterion 2

**Related**:
- Related task or issue
- Documentation to update
```

## Adding Process

### Step 1: Detect Appropriate Level
Use `detect-level` to determine where task belongs.

### Step 2: Classify Task Scope
- **Global**: Affects all profiles
- **Profile**: Profile-specific
- **Project**: Project-specific
- **Agent**: Agent-specific

### Step 3: Create or Append to Task File
```bash
# Check if task file exists
task_file="${level_path}/docs/tasks/${level}.md"

if [ ! -f "$task_file" ]; then
  # Create with header
  echo "# Tasks - ${level}" > "$task_file"
fi

# Append new task
echo "" >> "$task_file"
cat <<EOF >> "$task_file"
## $(date +%Y-%m-%d) - ${task_title}

**Priority**: ${priority}
**Status**: Pending
**Context**: ${context}

**Task**:
${description}

**Acceptance Criteria**:
- [ ] ${criterion1}
EOF
```

### Step 4: Update Parent If Needed
If task affects parent level, add reference:
```markdown
# Profile tasks.md

## Cross-Reference
- See global: ../docs/tasks/global.md
- See project: my-project/docs/tasks/project.md
```

## Integration with Other Systems

### Jira Integration (work profile)
For work profile tasks, optionally create Jira ticket:
```bash
# Create Jira issue if work profile
if [ "$profile" = "work" ]; then
  # Use Jira MCP to create issue
  # Link task file to Jira ticket
fi
```

### OmniFocus Integration (personal)
For personal tasks, optionally add to OmniFocus:
```bash
# Add to OmniFocus if requested
if [ "$add_to_omnifocus" = "true" ]; then
  # Use OmniFocus MCP
fi
```

## Task Management

### List Tasks
```bash
# List all pending tasks
grep -r "Status.*: Pending" ~/Projects/*/docs/tasks/
```

### Update Task Status
```bash
# Mark task as completed
sed -i '' 's/Status: Pending/Status: Completed/' task_file
```

### Archive Completed
```bash
# Move completed tasks to archive
# Keep tasks.md clean
```

## Integration

Invoked by:
- `/add-task` command
- Project planning workflows
- Issue tracking
- Task management reviews
