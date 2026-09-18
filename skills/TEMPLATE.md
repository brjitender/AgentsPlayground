# SKILL TEMPLATE

Copy this file and fill in all sections marked with `[TODO]`.

---

```yaml
---
name: [TODO: SkillName]
version: 1.0.0
description: [TODO: One-line description of what this skill does]
category: development | testing | documentation | deployment | [other]
complexity: low | medium | high
time_estimate: "[TODO: e.g., 15 min]"
tools_required: 
  - [TODO: tool names]
  - Example: edit, run, search/codebase
agents_who_call_this: 
  - [TODO: Agent names]
  - Example: Developer, Reviewer
dependencies: []
success_metrics:
  - [TODO: How to measure success]
---
```

## Overview

[TODO: Clear, detailed description of what this skill does]

**Example:**
- Input: [TODO]
- Process: [TODO]  
- Output: [TODO]

## When to Use This Skill

[TODO: Specific scenarios and conditions where this skill is applicable]

## How It Works

### Step 1: [TODO: First step name]
- [TODO: What happens]
- [TODO: Tools used]
- [TODO: Output]

### Step 2: [TODO: Second step name]
- [TODO: What happens]
- [TODO: Tools used]
- [TODO: Output]

### Step 3: [TODO: Continue steps]
[TODO: Add more steps as needed]

## Input/Output Specification

### Input
```
Type: [TODO: e.g., array of file paths]
Example:
[TODO: Provide real example input]

Schema:
{
  [TODO: Define expected input structure]
}
```

### Output
```
Type: [TODO: e.g., structured violation report]
Example:
[TODO: Provide real example output]

Schema:
{
  [TODO: Define expected output structure]
}
```

## Error Handling

### Error 1: [TODO: Potential error scenario]
- **Cause**: [TODO]
- **Impact**: [TODO]
- **Recovery**: [TODO]

### Error 2: [TODO: Another potential error]
- **Cause**: [TODO]
- **Impact**: [TODO]
- **Recovery**: [TODO]

[TODO: Add more error scenarios as needed]

## Edge Cases

1. **[TODO: Edge case description]**
   - How to handle: [TODO]
   - Expected output: [TODO]

2. **[TODO: Another edge case]**
   - How to handle: [TODO]
   - Expected output: [TODO]

[TODO: Add more edge cases as needed]

## Example Execution

### Example 1: [TODO: Scenario name]

**Input:**
```
[TODO: Real input example]
```

**Process:**
```
1. [TODO: What happens]
2. [TODO: What happens]
3. [TODO: What happens]
```

**Output:**
```
[TODO: Real output example]
```

### Example 2: [TODO: Another scenario]

[TODO: Add more examples as needed]

## Testing Strategy

### Test 1: Happy Path
- **Setup**: [TODO]
- **Action**: [TODO]
- **Expected**: [TODO]
- **Verify**: [TODO]

### Test 2: Error Path
- **Setup**: [TODO]
- **Action**: [TODO]
- **Expected**: [TODO]
- **Verify**: [TODO]

### Test 3: Edge Case
- **Setup**: [TODO]
- **Action**: [TODO]
- **Expected**: [TODO]
- **Verify**: [TODO]

[TODO: Add more tests as needed, aim for 5-10 total]

## Performance Considerations

- **Typical execution time**: [TODO]
- **Scalability**: [TODO]
- **Resource usage**: [TODO]
- **Optimization opportunities**: [TODO]

## Dependencies & Prerequisites

**Required tools:**
- [TODO: List]

**Required skills:**
- [TODO: List]

**Required setup:**
- [TODO: List]

## Known Limitations

1. [TODO]
2. [TODO]
3. [TODO]

## Future Enhancements

- [ ] [TODO: Enhancement 1]
- [ ] [TODO: Enhancement 2]
- [ ] [TODO: Enhancement 3]

## Integration Notes

**Where this skill is used:**
- Agent: [TODO: Which agent(s)]
- Plugins: [TODO: Which plugin(s)]
- Workflows: [TODO: Which workflows]

**How agents call this skill:**
```
Agent calls: Use [SkillName] skill
Input: [TODO]
Expected output: [TODO]
Error handling: [TODO]
```

## Troubleshooting

**Problem**: [TODO]
**Solution**: [TODO]

**Problem**: [TODO]
**Solution**: [TODO]

[TODO: Add more troubleshooting items as needed]

## Version History

### v1.0.0 (Initial Release)
- [TODO: Release notes]

## Authors & Contributors

- Created by: [TODO]
- Last updated: [TODO]
- Maintained by: [TODO]

---

## Checklist for Completion

Before submitting this skill:

- [ ] All [TODO] sections filled in
- [ ] At least 3 test cases written
- [ ] All error scenarios covered
- [ ] At least 2 execution examples provided
- [ ] Documentation is clear to someone unfamiliar with the code
- [ ] Performance notes documented
- [ ] Integration points identified
- [ ] Tested with actual agent calling it
- [ ] Ready for peer review

---

**Need help?** Check `docs/SKILL_DEVELOPMENT_GUIDE.md`
