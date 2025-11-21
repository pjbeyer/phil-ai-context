# agents-context-system

Claude Code plugin for hierarchical agent infrastructure management, AGENTS.md optimization, MCP configuration, and smart context loading.

## Project Purpose

Optimize AI context management across hierarchical development environments (global/profile/project/agent), reduce token usage, and provide intelligent context loading strategies for Claude Code.

## Architecture

### Core Components

**Hierarchy System**: Four-level organization (global → profile → project → agent) with profile detection, level detection, and cross-level navigation.

**Optimization Engine**: Token-efficient AGENTS.md files, MCP configuration optimization, settings management with priority-based recommendations.

**Context Loading**: Smart loading strategies that load only relevant context based on hierarchy level, reducing token overhead by up to 40%.

**Task Management**: Hierarchy-aware task tracking with automatic level placement and context-aware organization.

### Technology Stack

- **Language**: Bash scripts, JSON configuration
- **Integration**: Claude Code plugin system
- **Storage**: Hierarchical file system, JSON configs
- **Commands**: 4 slash commands (optimize-agents, optimize-mcp, optimize-settings, add-task)

## Skill Registry

**Optimization (3)**: optimize-agents-context, optimize-mcp-config, optimize-settings
**Hierarchy (3)**: detect-profile, detect-level, navigate-hierarchy
**Context (2)**: load-agents-context, analyze-context-usage
**Tasks (2)**: add-task, manage-tasks

See: skills/*/SKILL.md for detailed documentation

## Skill Routing

**For AGENTS.md optimization**: Use `optimize-agents-context`
**For MCP setup optimization**: Use `optimize-mcp-config`
**For settings consolidation**: Use `optimize-settings`
**For profile detection**: Use `detect-profile`
**For level detection**: Use `detect-level`
**For cross-level navigation**: Use `navigate-hierarchy`
**For smart context loading**: Use `load-agents-context`
**For token analysis**: Use `analyze-context-usage`
**For task creation**: Use `add-task`
**For task management**: Use `manage-tasks`

## Commands

### `/agents-context-system:optimize-agents`
Optimize AGENTS.md files across hierarchy for token efficiency.

**Target budgets**: Global (8-12k), Profile (5-8k), Project (4-6k), Agent (2-6k chars)

### `/agents-context-system:optimize-mcp`
Optimize MCP server configuration and documentation across profiles.

### `/agents-context-system:optimize-settings`
Optimize Claude Code settings with prioritized recommendations (HIGH/MEDIUM/LOW) and interactive approval.

**Modes**: Global (all profiles), Project (current project), User (user-level only)

### `/agents-context-system:add-task`
Add task to hierarchy-aware task management with automatic level placement.

## Integration Patterns

### With agents-documentation-suite
- AGENTS.md optimization for machine docs
- Token budget enforcement
- Hierarchical documentation coordination

### With agents-learning-system
- Learnings captured at appropriate hierarchy level
- Context-aware learning storage

### With workflow system
- Profile detection for workflow commands
- Hierarchy-aware task routing

## Token Efficiency Strategy

### Smart Context Loading
**Agent Level**: Load agent → project → profile (skip global) = 40% savings
**Project Level**: Load project → profile → global = full context
**Profile Level**: Load profile → global = two levels
**Global Level**: Load global only = minimal

### Content Extraction
Move detailed content from AGENTS.md to docs/:
- Procedures → docs/workflows/
- Examples → docs/examples/
- Architecture → docs/architecture/
- References → docs/reference/

### Hierarchical References
Reference parent level content instead of duplicating. Example:
```markdown
# BEFORE (Profile AGENTS.md)
Git configuration: [500 chars duplicated from global]

# AFTER (Profile AGENTS.md)
Git configuration: See global AGENTS.md
```

## Profile Standards

**pjbeyer**: Professional, client-focused, 4-6k docs, 80%+ coverage
**work**: Enterprise, security-first, 3-5k docs, 90%+ critical
**play**: Experimental, 2-4k docs, minimal tests
**home**: Practical, 2-4k docs, task-oriented

See: config/profile-definitions.json

## Configuration Files

**hierarchy-config.json**: Token budgets, loading strategies, hierarchy principles
**profile-definitions.json**: Profile purposes, standards, detection rules
**context-rules.json**: Optimization rules, content classification
**settings-rules.json**: Settings optimization rules, security patterns, priority classification

## Key Decisions

**Decision**: Use four-level hierarchy (not three or five)
**Rationale**: Balances granularity with complexity. Agent level needed for multi-agent projects.
**Date**: 2025-10-15

**Decision**: Skip global context at agent level
**Rationale**: 40% token savings, agent work rarely needs global patterns.
**Date**: 2025-10-20

**Decision**: Priority-based settings approval (HIGH/MEDIUM/LOW)
**Rationale**: Balance user control with automation. HIGH for security, LOW for safe changes.
**Date**: 2025-11-20

## Documentation

**README.md**: Human-readable overview, installation, examples
**docs/plans/**: Design and implementation plans
**docs/architecture/**: System architecture (TBD)
**config/**: Configuration files with inline documentation

## Development Status

**Version**: 1.0.5
**Status**: Active development
**Phase**: Optimize-settings Phase 1 complete, Phase 2 planned
**Next**: Settings modification and documentation generation

## Repository

https://github.com/pjbeyer/agents-context-system
