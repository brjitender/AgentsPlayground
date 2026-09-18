# PLUGIN TEMPLATE

Copy this file and fill in all sections marked with `[TODO]`.

---

```yaml
---
name: [TODO: PluginName]
version: 1.0.0
description: [TODO: One-line business outcome this plugin delivers]
category: feature-delivery | bug-fixing | documentation | deployment | [other]
invocation: /[TODO: plugin-name-lowercase]
execution_plan:
  stages:
    - name: [TODO: Stage 1 name]
      agents: [[TODO: Agent names]]
      gate: [TODO: none | human_approval | auto_gate]
    - name: [TODO: Stage 2 name]
      agents: [[TODO: Agent names]]
      gate: [TODO]
    [TODO: Add more stages]
configuration:
  strict_mode: true | false
  retry_limit: [TODO: number]
  timeout_minutes: [TODO: number]
  approval_gates: [[TODO: gate names]]
  parallel_execution: true | false
success_metrics:
  - [TODO: How success is measured]
  - [TODO: Add more metrics]
---
```

## Overview

[TODO: Clear description of this plugin's business value]

**What it does:**
[TODO: Clear, concise explanation]

**Who uses it:**
[TODO: Target users/roles]

**Time required:**
[TODO: Typical execution time]

## Business Value

[TODO: Why this plugin matters to the organization]

**Problem it solves:**
- [TODO]
- [TODO]

**Benefits:**
- [TODO]
- [TODO]

## Workflow Stages

### Stage 1: [TODO: Stage Name]

**Agents involved**: [TODO]
**Duration**: [TODO]

**What happens:**
1. [TODO]
2. [TODO]
3. [TODO]

**Gate type**: [TODO: human_approval | auto_gate | none]
**Gate criteria**: [TODO]

**Inputs:**
- [TODO]

**Outputs:**
- [TODO]

### Stage 2: [TODO: Stage Name]

[TODO: Repeat structure for each stage]

### Final Stage: [TODO: Usually Verification/Merge]

[TODO: Last stage definition]

## How to Invoke

### Syntax
```
/[TODO: plugin-name] "[TODO: required parameter]"
```

### Example
```
/[TODO: plugin-name] "Add user authentication to login page"
```

### Parameters
- [TODO: Parameter 1]: [TODO: Description]
- [TODO: Parameter 2]: [TODO: Description]

## Input Specification

```
Type: [TODO: e.g., feature requirement, bug description]
Format: [TODO: e.g., markdown, structured text]
Required fields:
  - [TODO]
  - [TODO]
Optional fields:
  - [TODO]

Example:
[TODO: Real example input]
```

## Expected Output

```
Type: [TODO: e.g., merged pull request, production deployment]
Format: [TODO: e.g., GitHub PR link, deployment report]

Example:
[TODO: Real example output]
```

## Configuration Options

```yaml
# Override defaults if needed
strict_mode: [TODO: true/false explanation]
retry_limit: [TODO: number of retries]
timeout_minutes: [TODO: time limit]
parallel_execution: [TODO: true/false]
```

## Approval Gates

### Gate 1: [TODO: Gate Name]
- **Triggered after**: [TODO: Which stage]
- **Criteria**: [TODO]
- **Approver**: [TODO]
- **Timeout**: [TODO]

### Gate 2: [TODO: Another gate]

[TODO: Add more gates as needed]

## Example Workflows

### Example 1: [TODO: Scenario Name]

**Input:**
```
[TODO: Real example input]
```

**Stages executed:**
1. Clarifier → Produces acceptance criteria
2. Developer → Writes code
3. Reviewer → Reviews code
4. Test → Runs tests
5. Docs → Updates documentation
6. Checklist → Verifies all criteria

**Output:**
```
[TODO: Real example output]
```

**Time taken:** [TODO]
**Status**: [TODO: Success/Partial/Failed]

### Example 2: [TODO: Different Scenario]

[TODO: Add more examples]

## Error Handling

### Error 1: [TODO: Potential Error]
- **When it occurs**: [TODO]
- **Impact**: [TODO]
- **Recovery**: [TODO]
- **Retry policy**: [TODO]

### Error 2: [TODO: Another Error]

[TODO: Add more error scenarios]

## Success Criteria

- [ ] [TODO: Criterion 1]
- [ ] [TODO: Criterion 2]
- [ ] [TODO: Criterion 3]

[TODO: Add more criteria as needed]

## Testing Strategy

### Test 1: Happy Path
- **Setup**: [TODO]
- **Input**: [TODO]
- **Expected**: [TODO]
- **Verify**: [TODO]

### Test 2: With Approval Gate Rejection
- **Setup**: [TODO]
- **Input**: [TODO]
- **Expected**: [TODO]
- **Verify**: [TODO]

### Test 3: With Stage Failure
- **Setup**: [TODO]
- **Input**: [TODO]
- **Expected**: [TODO]
- **Verify**: [TODO]

[TODO: Add more tests]

## Performance & Scalability

**Typical execution time**: [TODO]
**Concurrent limit**: [TODO]
**Resource requirements**: [TODO]
**Scalability notes**: [TODO]

## Integration Points

**Agents involved:**
- [TODO]

**Skills called:**
- [TODO]

**Tools used:**
- [TODO]

**External systems:**
- [TODO]

## Monitoring & Observability

**Metrics to track:**
- [TODO]
- [TODO]

**Logs to capture:**
- [TODO]
- [TODO]

**Dashboards:**
- [TODO]

## Troubleshooting

**Problem**: [TODO]
**Diagnosis**: [TODO]
**Solution**: [TODO]

[TODO: Add more troubleshooting items]

## Known Limitations

1. [TODO]
2. [TODO]
3. [TODO]

## Future Enhancements

- [ ] [TODO: Enhancement 1]
- [ ] [TODO: Enhancement 2]

## Dependencies & Prerequisites

**Required agents:**
- [TODO]

**Required skills:**
- [TODO]

**Required tools:**
- [TODO]

**External dependencies:**
- [TODO]

## Maintenance

**Regular tasks:**
- [TODO]
- [TODO]

**Update frequency:** [TODO]
**Maintenance owner:** [TODO]

## Version History

### v1.0.0 (Initial Release)
- [TODO: Release notes]

## Authors & Contributors

- Created by: [TODO]
- Last updated: [TODO]
- Maintained by: [TODO]

---

## Checklist for Completion

Before submitting this plugin:

- [ ] All [TODO] sections filled in
- [ ] Workflow stages clearly defined
- [ ] Approval gates configured
- [ ] At least 2 example workflows provided
- [ ] All error scenarios covered
- [ ] Success criteria documented
- [ ] Performance requirements met
- [ ] Integration points identified
- [ ] Testing completed
- [ ] Documentation is clear
- [ ] Ready for peer review

---

**Need help?** Check `docs/PLUGIN_DEVELOPMENT_GUIDE.md`
