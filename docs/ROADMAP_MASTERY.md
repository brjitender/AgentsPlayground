# 🚀 Custom Agents, Skills & Plugins Mastery Roadmap

## Your Current Architecture

You have built a **multi-agent orchestration system** with:
- **BrainBox**: Central orchestrator managing workflow
- **Specialized Agents**: Clarifier, Developer, Reviewer, Test, Docs, Checklist
- **Goal**: Automate complex development workflows with human approval gates

---

## 📊 The Ecosystem Pyramid

```
┌─────────────────────────────────────────────────────┐
│                   PLUGINS                           │
│  (Higher-order automations, packaged skills)        │
├─────────────────────────────────────────────────────┤
│                   SKILLS                            │
│  (Reusable task modules, callable by agents)        │
├─────────────────────────────────────────────────────┤
│                   AGENTS                            │
│  (Specialized reasoning engines + tools)            │
├─────────────────────────────────────────────────────┤
│                   TOOLS                             │
│  (edit, search, runTests, read, etc.)               │
└─────────────────────────────────────────────────────┘
```

---

## 🎯 Phase 1: Skills Fundamentals (Weeks 1-2)

### What Skills Are
A **skill** is a documented, reusable task module that agents can invoke. Think of it as a specialized workflow or capability.

### Anatomy of a Skill

```yaml
---
name: SkillName
description: Clear description of what this skill does
category: development | testing | documentation | deployment | etc
preconditions: "What must be true before this runs"
tools: ['edit', 'search/codebase', 'runTests']
agents: ['Developer', 'Test']  # Who can call this
complexity: low | medium | high
time_estimate: "5 min" | "20 min" | "1 hour"
---

# Skill Name

## What this does
[Detailed explanation]

## When to use it
[Use cases and triggers]

## How it works
[Step-by-step process]

## Input requirements
[What the caller must provide]

## Output
[What the skill returns]

## Edge cases
[Known limitations and edge cases]
```

### Phase 1 Tasks

**Week 1: Create your first 3 skills**

1. **Skill: Code-Quality-Check**
   - Purpose: Automated linting + style validation
   - Used by: Reviewer agent
   - Tools: `search/codebase`, `run` (lint command)
   - Input: file paths
   - Output: structured violations list

2. **Skill: Test-Coverage-Report**
   - Purpose: Measure test coverage against requirement
   - Used by: Test agent
   - Tools: `runTests`, `search/codebase`
   - Input: test suite path
   - Output: coverage % + gaps

3. **Skill: Generate-CHANGELOG**
   - Purpose: Auto-generate changelog entries
   - Used by: Docs agent
   - Tools: `search/codebase`, `edit`
   - Input: commit range or changes list
   - Output: formatted changelog entry

**Week 2: Intermediate skills with state**

4. **Skill: Requirement-to-Tests** (Bidirectional)
   - Parses requirement → generates test cases
   - Used by: Test agent as skill
   - Tools: `search/codebase`, `edit`

5. **Skill: Detect-Breaking-Changes**
   - Analyzes diff for API/behavior changes
   - Used by: Reviewer agent
   - Tools: `search/codebase`, `search/usages`

---

## 🎯 Phase 2: Skills in Practice (Weeks 3-4)

### How BrainBox Will Call Skills

```markdown
[In BrainBox thinking/planning]
Developer needs to check code quality first.
→ Invoke skill: Code-Quality-Check
  Input: ["src/newFeature.ts", "tests/newFeature.test.ts"]
  Result: { blocking: [], shouldFix: [{line: 12, issue: "unused var"}] }
→ Pass result to Reviewer for context
```

### Integrating Skills into Your Agents

Each agent description should reference which skills it calls:

```yaml
---
name: Reviewer
description: Code quality review using automated skills
tools: ['search/codebase', 'search/usages', 'read/terminalLastCommand']
skills: ['Code-Quality-Check', 'Detect-Breaking-Changes']  # NEW
agents_it_calls: []
---
```

### Phase 2 Tasks

1. **Refactor Developer agent** to use:
   - `Generate-Unit-Tests` skill
   - `Code-Quality-Check` skill
   - `Test-Coverage-Report` skill

2. **Refactor Reviewer agent** to use:
   - `Code-Quality-Check` skill
   - `Detect-Breaking-Changes` skill
   - `Security-Scan` skill (create this)

3. **Refactor Docs agent** to use:
   - `Extract-API-Changes` skill (create this)
   - `Generate-CHANGELOG` skill
   - `Generate-README-Update` skill (create this)

4. **Create skill catalog**
   - Document all skills with examples
   - Create a `SKILLS.md` index file
   - Add cross-references

---

## 🎯 Phase 3: Plugins (Weeks 5-6)

### What Plugins Are
A **plugin** is a **packaged collection of skills** that solve a larger business problem. It's a higher-level abstraction than a skill.

### Plugin Anatomy

```yaml
---
name: PluginName
version: 1.0.0
description: High-level problem this solves
author: Your Name
tags: [development, testing, documentation]
agents_required: [Developer, Test, Reviewer]
skills_bundled: 
  - Code-Quality-Check
  - Test-Coverage-Report
  - Generate-CHANGELOG
entry_point: BrainBoxOrchestrator
configuration:
  strictMode: true
  retryLimit: 3
  autoApproveThreshold: low-risk
---

# Plugin: Feature-Delivery-Pipeline

## What this plugin does
End-to-end workflow: requirement → code → tests → review → docs → sign-off

## Prerequisites
- GitHub repo with agent access
- Test framework configured
- Documentation templates in place

## How to invoke
```
/feature-delivery {requirement-text}
```

## What it orchestrates
1. Clarifier analyzes requirement
2. Developer implements (using skills)
3. Reviewer auto-checks (using skills)
4. Test suite runs (using skills)
5. Docs update (using skills)
6. Checklist verifies (using skills)
7. Human approval gate
8. Auto-merge and notify

## Configuration options
- strictMode: require human approval on all changes
- autoApproveThreshold: auto-approve if low-risk

## Monitoring & Logs
Plugin tracks: time per agent, retry counts, failure reasons
```

### Phase 3 Tasks

**Create these 3 plugins:**

1. **Plugin: Feature-Delivery-Pipeline**
   - Bundles: All skills
   - Entry: BrainBox
   - Use case: New feature from requirement to merge

2. **Plugin: Bug-Fix-Express**
   - Bundles: Developer, Reviewer, Test (no docs update)
   - Entry: BrainBox
   - Use case: Low-risk bug fixes with fast-track approval
   - Config: autoApproveThreshold: low-risk

3. **Plugin: Documentation-Sync**
   - Bundles: Docs agent + related skills
   - Entry: Docs agent
   - Use case: Keep docs in sync with code after changes

---

## 🎯 Phase 4: Advanced Patterns (Weeks 7-8)

### Plugin Composition & Chaining

```yaml
# Meta-Plugin: Entire-Release-Cycle

plugins_chained:
  - Feature-Delivery-Pipeline (for each feature)
  - Documentation-Sync (after all features done)
  - Release-Notes-Generator (bundles Docs skills)
  - Deploy-To-Staging (new plugin)
  - Run-Integration-Tests (new plugin)
  - Production-Checklist (new plugin)

orchestration:
  parallelizable: [Feature-Delivery-Pipeline x N]
  sequential: [Documentation-Sync, Release-Notes, Deploy, Tests, Checklist]
  approval_gates: [before Deploy-To-Staging, before Production]
```

### Conditional Skill Invocation

```yaml
# Skill: Risk-Aware-Review
# Invokes different review intensity based on change type

on_input:
  if change_type == "schema_migration":
    invoke: [Security-Scan, Performance-Test]
  elif change_type == "config_only":
    invoke: [Code-Quality-Check]  # lighter touch
  else:
    invoke: [Code-Quality-Check, Detect-Breaking-Changes]
```

### Phase 4 Tasks

1. **Create deployment plugins**:
   - `Deploy-To-Staging` plugin
   - `Deploy-To-Production` plugin
   - `Rollback-Procedure` plugin

2. **Create conditional skill logic**:
   - Schema change detection → heavier review
   - Config change → lighter review
   - API change → security + breaking change scan

3. **Create observability**:
   - Plugin execution tracking
   - Skill performance metrics
   - Agent decision logging

---

## 🎯 Phase 5: Mastery & Customization (Weeks 9+)

### What You'll Build

1. **Domain-Specific Plugins**
   - Backend API Development Plugin
   - Frontend Component Development Plugin
   - Database Migration Plugin
   - DevOps/Infrastructure Plugin

2. **Team Workflows**
   - Code-review-on-demand skill
   - Dependency-update skill
   - Architecture-review skill

3. **Analytics & Insights**
   - Plugin success rates
   - Agent performance tracking
   - Skill reusability metrics

4. **Self-Improving System**
   - Failed task analysis
   - Agent calibration
   - Retry pattern learning

---

## 📋 Skills Checklist Template

Use this to design each skill:

```markdown
# Skill: [Name]

## Problem Statement
What workflow problem does this solve?

## Inputs
- [input1]: type, required/optional
- [input2]: type, required/optional

## Process
1. Step 1
2. Step 2
3. Step 3

## Outputs
- success: [output1, output2]
- failure: [error conditions]

## Integration Points
- Called by: [Agent1, Agent2]
- Calls skills: [Skill1, Skill2]
- Calls tools: [tool1, tool2]

## Testing Strategy
- Happy path test
- Edge cases
- Failure modes

## Success Criteria
- Completes in < X time
- Output has < Y errors
- Human approval rate > Z%
```

---

## 🔧 Development Workflow

### In VS Code with GitHub Copilot

1. **Create skill branch**: `feature/skill-{skillname}`
2. **Write skill definition**: `skills/{skillname}.md`
3. **Let Copilot help with**:
   - Generating examples
   - Creating test cases
   - Documenting edge cases
4. **Test skill integration**:
   - Create a test harness agent
   - Call skill from test agent
   - Verify outputs
5. **PR and merge**: Ask Copilot to review against skill checklist

### GitHub Structure

```
AgentsPlayground/
├── .github/
│   └── workflows/
│       ├── skill-test.yml
│       ├── plugin-validate.yml
│       └── agent-integration.yml
├── agents/
│   ├── BrainBox.agent.md
│   ├── Developer.agent.md
│   └── ... (all agents)
├── skills/
│   ├── code-quality-check.md
│   ├── test-coverage-report.md
│   ├── generate-changelog.md
│   └── ... (all skills)
├── plugins/
│   ├── feature-delivery-pipeline.md
│   ├── bug-fix-express.md
│   └── ... (all plugins)
├── docs/
│   ├── SKILLS.md (index)
│   ├── PLUGINS.md (index)
│   ├── AGENTS.md (index)
│   └── ARCHITECTURE.md
├── examples/
│   ├── skill-usage-examples.md
│   └── plugin-invocation-examples.md
└── README.md
```

---

## 🎓 Learning Path Summary

| Phase | Duration | Focus | Output |
|-------|----------|-------|--------|
| 1 | Weeks 1-2 | Understand skills | 5 basic skills |
| 2 | Weeks 3-4 | Integrate skills into agents | Refactored agents + skill catalog |
| 3 | Weeks 5-6 | Create plugins | 3 packaged plugins |
| 4 | Weeks 7-8 | Advanced patterns | Conditional logic, composition |
| 5 | Weeks 9+ | Mastery | Domain-specific, self-improving |

---

## 🔥 Quick Wins to Start Today

1. **Create `skills/TEMPLATE.md`**
   - Copy the skill anatomy above
   - Customize for your codebase patterns

2. **Create `SKILLS_ROADMAP.md`**
   - List skills you'll need for Phase 1
   - Assign to agents that will use them

3. **Create first skill: `Code-Quality-Check`**
   - Read existing linting rules in your codebase
   - Document the skill
   - Show example output

4. **Update agent descriptions**
   - Add `skills:` field
   - Add `skill_examples:` field
   - Cross-link to skill definitions

5. **GitHub: Set up branch protection**
   - Require plugin validation before merge
   - Require skill documentation review

---

## 💡 Key Insights

### Skills vs Agents
- **Agent**: Reasoning engine that decides *what to do*
- **Skill**: Specific *how to do it* — pre-built, tested, documented

### Skills vs Plugins
- **Skill**: Atomic task (e.g., "run linter")
- **Plugin**: Orchestrated workflow (e.g., "entire feature delivery")

### Tools vs Skills vs Plugins
```
Tool (lowest level):        runTests, edit, search
Skill (mid level):          Test-Coverage-Report (uses runTests tool)
Plugin (highest level):     Feature-Delivery-Pipeline (chains multiple skills)
```

---

## Next Document: Skill Development Guide

See `SKILL_DEVELOPMENT_GUIDE.md` for hands-on examples.
