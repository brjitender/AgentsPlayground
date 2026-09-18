# Skills Directory

Reusable task modules that agents can call during execution.

## What are Skills?

Skills are focused, reusable task modules with:
- Clear inputs and outputs
- Well-defined responsibility
- Comprehensive error handling
- Full test coverage

## Skill Anatomy

```yaml
---
name: SkillName
version: 1.0.0
description: What this skill does
category: development | testing | documentation
complexity: low | medium | high
time_estimate: "15 min"
tools_required: [tool names]
agents_who_call_this: [agent names]
---
```

Plus sections for:
- Overview
- How It Works
- Input/Output Specification
- Error Handling
- Edge Cases
- Examples
- Testing Strategy

## Creating a New Skill

1. Copy `TEMPLATE.md`
2. Fill in all sections
3. Write test cases (5+ minimum)
4. Create PR

See `docs/SKILL_DEVELOPMENT_GUIDE.md` for detailed instructions.

## Skill Categories

**Development**
- Code-Quality-Check
- Detect-Breaking-Changes
- Generate-Unit-Tests

**Testing**
- Test-Coverage-Report
- Run-Integration-Tests

**Documentation**
- Generate-Changelog
- Extract-API-Changes

**Deployment**
- Pre-Deployment-Checks
- Rollback-Plan

## Phase 1 Skills (Weeks 1-2)

These 5 skills are the foundation:

1. **Code-Quality-Check**
   - Input: Code files
   - Output: Linter violations
   - Used by: Developer, Reviewer

2. **Test-Coverage-Report**
   - Input: Test files + requirements
   - Output: Coverage analysis
   - Used by: Test agent

3. **Generate-Changelog**
   - Input: Git commits
   - Output: Formatted changelog
   - Used by: Docs agent

4. **Detect-Breaking-Changes**
   - Input: Code diff
   - Output: Breaking changes list
   - Used by: Reviewer

5. **Security-Scan**
   - Input: Code files
   - Output: Security issues
   - Used by: Reviewer

## Adding Skills to Agents

After creating a skill, integrate it into agents:

```yaml
# In agent definition
skills_called:
  - name: Code-Quality-Check
    when: code_complete
    required_for: code_review
```

## Skill Performance

Track and monitor:
- Execution time
- Error rate
- Accuracy
- Usage frequency

See `monitoring/skill-performance.md`

## Best Practices

- **Single responsibility**: One clear job
- **Clear interface**: Well-defined inputs/outputs
- **Error handling**: Graceful failure
- **Testing**: Comprehensive test coverage
- **Documentation**: Clear examples

## Integration with Plugins

Skills are called by agents within plugins:

```
Plugin
  └─ Agent
      └─ Skill (reusable module)
          └─ Tool (edit, run, etc.)
```

---

Ready to create your first skill? Start with `docs/SKILL_DEVELOPMENT_GUIDE.md`

