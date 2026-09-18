# Plugins Directory

Orchestrated workflows that automate complete business processes.

## What are Plugins?

Plugins are coordinated multi-agent workflows with:
- Clear business outcome
- Multiple agents working together
- Approval gates and verification
- End-to-end process automation

## Plugin Anatomy

```yaml
---
name: PluginName
invocation: /plugin-name
execution_plan:
  stages:
    - name: Stage Name
      agents: [Agent Names]
      gate: none | human_approval | auto_gate
---
```

## Invocation

```bash
/plugin-name "input description"
```

## Phase 1 Plugins (Weeks 5-6)

Three core plugins to build:

1. **Feature-Delivery-Pipeline**
   - Full workflow: Requirement → Merge
   - Agents: All 7
   - Gates: 2 approval gates
   - Time: ~45 min

2. **Bug-Fix-Express**
   - Quick workflow: Bug → Fix
   - Agents: Developer, Reviewer, Test
   - Gates: Auto-approve
   - Time: ~15 min

3. **Documentation-Sync**
   - Auto docs: Code change → Updated docs
   - Agents: Docs, Checklist
   - Gates: Human approval
   - Time: ~20 min

## Creating a New Plugin

1. Copy `TEMPLATE.md`
2. Define all stages
3. Configure approval gates
4. Write examples
5. Create PR

See `docs/PLUGIN_DEVELOPMENT_GUIDE.md` for details.

## Plugin Workflow

```
User Input
  ↓
[Stage 1: Agents Work]
  ↓ [Gate 1: Approval]
[Stage 2: Agents Work]
  ↓ [Gate 2: Approval]
[Stage 3: Agents Work]
  ↓
Complete Output
```

## Best Practices

- **Clear stages**: Each stage has one clear purpose
- **Smart gates**: Approval gates at critical points
- **Error handling**: Robust failure recovery
- **Monitoring**: Track execution and metrics
- **Documentation**: Clear examples and use cases

---

Ready to create your first plugin? Start with `docs/PLUGIN_DEVELOPMENT_GUIDE.md`

