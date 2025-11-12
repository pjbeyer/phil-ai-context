---
name: load-agents-context
description: Smart context loading that loads only relevant AGENTS.md files based on hierarchy level and profile, optimizing startup performance.
---

# Smart Context Loading Skill

Load only relevant AGENTS.md files based on current hierarchy level, optimizing context usage and startup performance.

## Loading Strategy

### Agent Level
**Load**: agent → project → profile (skip global for efficiency)
```bash
/Users/pjbeyer/Projects/work/project/agents/scanner/AGENTS.md
/Users/pjbeyer/Projects/work/project/AGENTS.md
/Users/pjbeyer/Projects/work/AGENTS.md
```
**Skip global**: Agent work is specific, global context rarely needed

### Project Level
**Load**: project → profile → global
```bash
/Users/pjbeyer/Projects/work/project/AGENTS.md
/Users/pjbeyer/Projects/work/AGENTS.md
/Users/pjbeyer/Projects/AGENTS.md
```
**Full chain**: Projects may need cross-profile patterns

### Profile Level
**Load**: profile → global
```bash
/Users/pjbeyer/Projects/work/AGENTS.md
/Users/pjbeyer/Projects/AGENTS.md
```
**Two levels**: Profile work needs global context

### Global Level
**Load**: global only
```bash
/Users/pjbeyer/Projects/AGENTS.md
```
**Single file**: Working at global level

## Loading Process

### Step 1: Detect Current Level
Use `detect-level` and `detect-profile` skills to determine context.

### Step 2: Determine Load List
Based on level, create ordered list of AGENTS.md files to load.

### Step 3: Verify Files Exist
Check each file exists before attempting load:
```bash
for file in "${load_list[@]}"; do
  if [ -f "$file" ]; then
    echo "Loading: $file"
  else
    echo "Skipping (not found): $file"
  fi
done
```

### Step 4: Load in Order
Load from most specific to least specific (bottom-up):
- Agent context first (most specific)
- Project context next
- Profile context
- Global context last (most general)

### Step 5: Track Loaded Context
Record what was loaded for debugging and optimization:
```json
{
  "loaded": [
    {"path": "agent/AGENTS.md", "size": 3421},
    {"path": "project/AGENTS.md", "size": 5234"},
    {"path": "profile/AGENTS.md", "size": 6789}
  ],
  "totalChars": 15444,
  "level": "agent",
  "profile": "work"
}
```

## Optimization Benefits

### Token Savings
**Before** (load everything):
- Global: 10k chars
- Profile: 7k chars
- Project: 5k chars
- Agent: 3k chars
- **Total**: 25k chars

**After** (smart loading at agent level):
- Agent: 3k chars
- Project: 5k chars
- Profile: 7k chars
- **Total**: 15k chars (40% savings)

### Startup Performance
- Fewer files to read
- Less token processing
- Faster session initialization
- Better focus on relevant context

## Configuration

**Context loading rules** (from `config/context-rules.json`):
```json
{
  "loadingStrategy": {
    "agent": ["agent", "project", "profile"],
    "project": ["project", "profile", "global"],
    "profile": ["profile", "global"],
    "global": ["global"]
  },
  "maxContextSize": {
    "agent": 20000,
    "project": 25000,
    "profile": 18000,
    "global": 12000
  }
}
```

## Error Handling

**Missing files**:
- Skip gracefully
- Log warning
- Continue with available context
- Don't fail session start

**Oversized context**:
- Warn if over budget
- Suggest optimization
- Don't block loading

## Integration

This skill is invoked by:
- Session start hook (smart-context-loader.js)
- Manual context reload
- Context switching between projects
- Profile changes
