---
name: detect-level
description: Detect hierarchy level (global/profile/project/agent) from current directory path.
---

# Hierarchy Level Detection Skill

Determine the hierarchy level based on directory depth and structure.

## Hierarchy Levels

### Global
**Path**: `/Users/pjbeyer/Projects`
**Indicator**: At Projects root
**AGENTS.md**: `/Users/pjbeyer/Projects/AGENTS.md`

### Profile
**Path**: `/Users/pjbeyer/Projects/{profile}`
**Indicator**: Direct child of Projects, one of: pjbeyer/work/play/home
**AGENTS.md**: `{profilePath}/AGENTS.md`

### Project
**Path**: `/Users/pjbeyer/Projects/{profile}/{project}`
**Indicator**: Two levels below Projects, has AGENTS.md or is a repository
**AGENTS.md**: `{projectPath}/AGENTS.md`

### Agent
**Path**: `/Users/pjbeyer/Projects/{profile}/{project}/agents/{agent}`
**Indicator**: Inside agents/ directory
**AGENTS.md**: `{agentPath}/AGENTS.md`

## Detection Logic

```bash
pwd_path="$PWD"
projects_root="/Users/pjbeyer/Projects"

# Count depth from Projects root
depth=$(echo "$pwd_path" | sed "s|$projects_root||" | tr -cd '/' | wc -c)

case $depth in
  0) echo "global" ;;
  1) echo "profile" ;;
  2)
    # Check if in agents/ subdirectory
    if [[ "$pwd_path" == */agents/* ]]; then
      echo "agent"
    else
      echo "project"
    fi
    ;;
  *)
    # Deeper than 2, likely agent or sub-project
    if [[ "$pwd_path" == */agents/* ]]; then
      echo "agent"
    else
      echo "project"
    fi
    ;;
esac
```

## Context Loading Strategy

**Based on level, load**:
- **Global**: Load global AGENTS.md only
- **Profile**: Load profile + global AGENTS.md
- **Project**: Load project + profile + global AGENTS.md
- **Agent**: Load agent + project + profile (skip global for efficiency)

## Return Format

```json
{
  "level": "project",
  "profile": "work",
  "path": "/Users/pjbeyer/Projects/work/my-project",
  "agentsMdPath": "/Users/pjbeyer/Projects/work/my-project/AGENTS.md",
  "parentPaths": {
    "profile": "/Users/pjbeyer/Projects/work",
    "global": "/Users/pjbeyer/Projects"
  }
}
```

## Integration

Invoked by:
- Profile detection (detect-profile)
- Context loading (load-agents-context)
- Navigation (navigate-hierarchy)
- Optimization (optimize-agents)
