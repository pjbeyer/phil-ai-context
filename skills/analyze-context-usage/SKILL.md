---
name: analyze-context-usage
description: Analyze context token usage, identify optimization opportunities, and provide recommendations for reducing context overhead.
---

# Context Usage Analysis Skill

Analyze AGENTS.md token usage and provide optimization recommendations.

## Analysis Process

### Step 1: Measure Current Usage

**Find and measure all AGENTS.md files**:
```bash
find ~/Projects -name "AGENTS.md" -type f -exec wc -c {} + | sort -rn
```

**Calculate token estimates**:
- Characters ÷ 4 ≈ tokens (rough estimate)
- 1 token ≈ 4 characters for English text

### Step 2: Compare Against Budgets

**Target budgets**:
| Level | Character Budget | Token Budget |
|-------|------------------|--------------|
| Global | 8-12k chars | 2-3k tokens |
| Profile | 5-8k chars | 1.25-2k tokens |
| Project | 4-6k chars | 1-1.5k tokens |
| Agent | 2-6k chars | 0.5-1.5k tokens |

**Flag overages**:
- Yellow: 10-25% over budget
- Red: 25%+ over budget

### Step 3: Identify Redundancy

**Check for duplication**:
- Same content in multiple levels
- Parent content copied to children
- Cross-level repetition

**Pattern detection**:
```bash
# Find common phrases across files
for file in $(find ~/Projects -name "AGENTS.md"); do
  # Extract significant phrases
  # Compare across files
  # Report duplicates
done
```

### Step 4: Analyze Content Types

**Classify content**:
- Essential (routing, capabilities, integration)
- Detailed (examples, procedures, background)
- Redundant (duplicate, outdated, verbose)

**Provide breakdown**:
```markdown
## work/AGENTS.md Analysis

**Size**: 8,234 characters (103% of budget)
**Estimated tokens**: ~2,058 tokens

**Content breakdown**:
- Essential: 3,500 chars (43%)
- Detailed: 3,800 chars (46%) ← Extract to docs/
- Redundant: 934 chars (11%) ← Remove or reference

**Recommendations**:
1. Extract MCP tool details to docs/mcp/ (save ~2k chars)
2. Reference global git config instead of duplicating (save ~500 chars)
3. Move examples to docs/examples/ (save ~1.3k chars)

**Projected size after**: 4,734 chars (59% of current, 94% of budget)
```

### Step 5: Cross-Level Analysis

**Check hierarchy health**:
- Does child duplicate parent content?
- Is information at correct level?
- Are references clear?

**Example findings**:
```markdown
## Hierarchy Issues Found

### Git Configuration (Duplicated)
- Found in: global, pjbeyer, work, play
- Size: ~500 chars each (total waste: 1,500 chars)
- **Fix**: Keep in global, reference from profiles

### MCP Servers (Wrong level)
- Found in: project AGENTS.md
- Should be: profile or global level
- **Fix**: Move to profile, reference from project
```

### Step 6: Generate Report

**Summary metrics**:
```markdown
## Context Usage Report - 2025-11-11

### Overall Statistics
- Total AGENTS.md files: 42
- Total characters: 187,432
- Estimated tokens: ~46,858
- Files over budget: 8 (19%)

### By Profile
- **pjbeyer**: 7 files, 34,234 chars (avg 4,891 chars/file)
- **work**: 23 files, 112,334 chars (avg 4,884 chars/file)
- **play**: 5 files, 18,432 chars (avg 3,686 chars/file)
- **global**: 1 file, 10,432 chars

### Optimization Potential
- Duplicate content: ~12,500 chars (7% of total)
- Detailed content to extract: ~45,000 chars (24% of total)
- Projected savings: ~57,500 chars (31% reduction)

### Priority Actions
1. Extract work profile MCP details (save ~15k chars)
2. Consolidate git configuration references (save ~2k chars)
3. Move examples to docs/ directories (save ~25k chars)
4. Remove outdated agent references (save ~5k chars)
```

## Recommendations Format

**For each file, provide**:
1. Current size vs budget
2. Content breakdown
3. Specific extraction opportunities
4. Expected savings
5. Priority level

## Integration

Invoked by:
- `/optimize-agents` command
- Quarterly optimization reviews
- Context budget compliance checks
- Performance troubleshooting
