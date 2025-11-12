---
name: detect-profile
description: Detect current profile from directory path (pjbeyer/work/play/home/global) and return profile-specific standards and settings.
---

# Profile Detection Skill

Detect the current profile from the working directory path and return profile-specific configuration.

## Detection Logic

```bash
# Get current directory
pwd

# Determine profile
case "$PWD" in
  */Projects/pjbeyer/*) echo "pjbeyer" ;;
  */Projects/work/*) echo "work" ;;
  */Projects/play/*) echo "play" ;;
  */Projects/home/*) echo "home" ;;
  */Projects) echo "global" ;;
  *) echo "unknown" ;;
esac
```

## Profile Definitions

### pjbeyer
- **Purpose**: Professional/consulting work
- **Standards**: 80% test coverage, comprehensive docs
- **Style**: Client-focused quality
- **Primary tools**: GitHub, Notion, professional MCP servers

### work
- **Purpose**: Enterprise/corporate (Flex)
- **Standards**: 90% test coverage (critical paths), slim docs (3-5k)
- **Style**: Security-first, enterprise compliance
- **Primary tools**: Jira, Flex GitHub, DryRun, Notion

### play
- **Purpose**: Experimental/learning
- **Standards**: Minimal test coverage, flexible quality
- **Style**: Quick iteration, learning-focused
- **Primary tools**: Whatever's being experimented with

### home
- **Purpose**: Personal/family
- **Standards**: Practical automation, simple docs
- **Style**: Accessible, non-technical
- **Primary tools**: Calendar, tasks, family organization

### global
- **Purpose**: Cross-profile patterns
- **Standards**: Universal applicability
- **Style**: Profile-agnostic
- **Scope**: Environment setup, global standards

## Return Format

```json
{
  "profile": "work",
  "level": "project",
  "path": "/Users/pjbeyer/Projects/work/my-project",
  "standards": {
    "testCoverage": "90%",
    "docStyle": "slim",
    "security": "enterprise"
  },
  "agentsMdPath": "/Users/pjbeyer/Projects/work/my-project/AGENTS.md",
  "docsPath": "/Users/pjbeyer/Projects/work/my-project/docs",
  "parentProfile": "/Users/pjbeyer/Projects/work/AGENTS.md"
}
```

## Integration

Invoked by:
- Other hierarchy skills (detect-level, navigate-hierarchy)
- Context loading (load-agents-context)
- Optimization workflows (optimize-agents, optimize-mcp)
