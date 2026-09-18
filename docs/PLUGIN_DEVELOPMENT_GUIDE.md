# 🔌 Plugin Development Guide

## Part 1: Plugin Architecture

### What's a Plugin?

A **plugin** is a self-contained, packaged workflow that solves a business problem by orchestrating agents and skills.

```
Plugin = Agents + Skills + Orchestration Logic + Configuration
```

### Plugin Layers

```
┌─────────────────────────────────────┐
│     Invocation Layer                │
│  `/feature-delivery {requirement}`  │
├─────────────────────────────────────┤
│     Orchestration Layer             │
│  BrainBox: route → retry → approve  │
├─────────────────────────────────────┤
│     Agent Layer                     │
│  Clarifier, Developer, Reviewer,    │
│  Test, Docs, Checklist              │
├─────────────────────────────────────┤
│     Skill Layer                     │
│  Code-Quality, Test-Coverage,       │
│  Generate-Changelog, etc.           │
├─────────────────────────────────────┤
│     Tool Layer                      │
│  edit, search, run, read, runTests  │
└─────────────────────────────────────┘
```

---

## Part 2: Plugin Anatomy

### Complete Plugin Template

```yaml
---
# Plugin Metadata
name: FeatureDeliveryPipeline
version: 1.0.0
description: Complete workflow from requirement to production deployment
category: workflow | utility | integration
status: stable | beta | experimental
authors: [Your Name, Collaborator]
tags: [feature-dev, high-throughput, human-approved]
created_at: 2024-01-15
last_updated: 2024-01-15

# What this plugin needs
requirements:
  - agents: [BrainBox, Clarifier, Developer, Reviewer, Test, Docs, Checklist]
  - skills: [Code-Quality-Check, Test-Coverage-Report, Detect-Breaking-Changes, Generate-Changelog]
  - tools: [edit, search, run, runTests, read]
  - infrastructure: [GitHub repo, CI/CD pipeline, test framework]

# How to invoke it
invocation:
  syntax: "/feature-delivery {requirement-text}"
  aliases: ["/feature", "/implement"]
  params:
    requirement:
      type: string
      description: Natural language description of feature
      required: true
    target_branch:
      type: string
      description: Branch to merge into
      default: main
    auto_approve_threshold:
      type: string
      enum: [low-risk, medium-risk, none]
      default: none

# Configuration
configuration:
  strict_mode: true  # Require human approval on all changes
  retry_limit: 3  # Max retries per agent before escalation
  timeout_minutes: 60  # Max time allowed per invocation
  approval_gates:
    - stage: pre_development  # After Clarifier
    - stage: pre_merge  # After all agents pass
    - stage: pre_deployment  # Before deploy (if included)

# What stages run and in what order
execution_plan:
  stages:
    - name: "Clarification"
      agents: [Clarifier]
      parallel: false
      gate: human_confirms_criteria
      
    - name: "Implementation"
      agents: [Developer]
      parallel: false
      gate: none
      
    - name: "Quality Gate 1: Code Review"
      agents: [Reviewer]
      parallel: false
      gate: none
      
    - name: "Quality Gate 2: Testing"
      agents: [Test]
      parallel: false
      gate: automatic
      
    - name: "Documentation"
      agents: [Docs]
      parallel: false
      gate: none
      
    - name: "Final Verification"
      agents: [Checklist]
      parallel: false
      gate: automatic
      
    - name: "Deployment"  # Optional
      agents: [DeploymentAgent]  # If you have it
      parallel: false
      gate: human_approves_deployment

  total_estimated_time: "45 minutes"
  parallelizable_stages: []  # None in this workflow, but could group stages

# Success criteria
success_criteria:
  - All agents complete without blocking issues
  - Checklist passes
  - Tests pass with >80% coverage
  - Documentation updated
  - Human approval granted

# Monitoring & observability
monitoring:
  metrics_tracked:
    - total_duration
    - duration_per_agent
    - retry_count_per_agent
    - approval_gate_delays
    - success_rate
  logs_retained: 90  # days
  retry_analysis: enabled

# Related plugins and skills
depends_on_plugins: []  # Can chain plugins
complements_plugins: [Documentation-Sync, Release-Notes-Generator]
uses_skills: 
  - Code-Quality-Check
  - Test-Coverage-Report
  - Detect-Breaking-Changes
  - Generate-Changelog

# Limitations & disclaimers
limitations: |
  - Cannot handle schema migrations (requires DBA review)
  - Cannot deploy to production (requires separate approval)
  - Requires passing security scan before approval

# Support & troubleshooting
troubleshooting:
  - issue: "Developer step times out"
    solution: "Break requirement into smaller pieces"
  - issue: "Test coverage below target"
    solution: "Check acceptance criteria are testable"

---

# Plugin: Feature-Delivery-Pipeline

## Overview

End-to-end workflow for shipping production-ready features. Automates requirement clarification, implementation, code review, testing, documentation, and verification.

## Problem It Solves

- **Before**: Unclear requirements → implementation → multiple review cycles → incomplete docs → missed edge cases
- **After**: Clear requirements → quality implementation → automated checks → complete docs → verified checklist

## Who Should Use This

- Engineers shipping new features
- Teams enforcing code quality
- Organizations with approval workflows
- Anyone tired of back-and-forth code reviews

## When to Use It

✅ New feature with clear requirements  
✅ Medium complexity work (not trivial bug, not major refactor)  
✅ Team has agents and skills configured  
✅ You want documented decisions  

❌ Simple one-line bug fix (use Bug-Fix-Express instead)  
❌ Exploratory/research work  
❌ Critical production hotfix (use Hotfix-Pipeline instead)  

---

## How It Works (Visual Flow)

```
┌─────────────────────────────────┐
│ 1. CLARIFICATION               │
│ Clarifier asks questions       │
│ ✓ Acceptance criteria locked   │
└────────────┬────────────────────┘
             │ (human confirms)
             ↓
┌─────────────────────────────────┐
│ 2. IMPLEMENTATION              │
│ Developer writes code + tests  │
│ Skills run:                    │
│   - Code-Quality-Check         │
│   - Test-Coverage-Report       │
└────────────┬────────────────────┘
             │
             ↓
┌─────────────────────────────────┐
│ 3. CODE REVIEW                 │
│ Reviewer checks quality        │
│ Skills run:                    │
│   - Code-Quality-Check         │
│   - Detect-Breaking-Changes    │
│   - Security-Scan              │
└────────────┬────────────────────┘
             │ (blocking issues?)
             ├─ YES → Loop to Developer
             └─ NO → Continue
             ↓
┌─────────────────────────────────┐
│ 4. TESTING                     │
│ Test agent verifies coverage   │
│ Skills run:                    │
│   - Test-Coverage-Report       │
│   - Requirement-to-Tests       │
└────────────┬────────────────────┘
             │ (gaps found?)
             ├─ YES → Loop to Developer
             └─ NO → Continue
             ↓
┌─────────────────────────────────┐
│ 5. DOCUMENTATION               │
│ Docs agent updates README,API  │
│ Skills run:                    │
│   - Extract-API-Changes        │
│   - Generate-CHANGELOG         │
│   - Generate-README-Update     │
└────────────┬────────────────────┘
             │
             ↓
┌─────────────────────────────────┐
│ 6. VERIFICATION                │
│ Checklist confirms all criteria│
│ met, all docs updated          │
└────────────┬────────────────────┘
             │ (all pass?)
             ├─ NO → Loop to specific agent
             └─ YES → Continue
             ↓
┌─────────────────────────────────┐
│ 7. APPROVAL GATE               │
│ Human reviews and approves     │
└────────────┬────────────────────┘
             │ (human approves?)
             ├─ NO → Request changes
             └─ YES → Continue
             ↓
┌─────────────────────────────────┐
│ 8. MERGE & NOTIFY              │
│ Auto-merge to main             │
│ Send notification to team      │
│ Close related issues/PRs       │
└─────────────────────────────────┘
```

---

## Step-by-Step Invocation

### Step 1: User Invokes Plugin

```
/feature-delivery "Add OAuth2 authentication with GitHub and Google providers"
```

### Step 2: BrainBox Orchestrates

```python
def execute_plugin(requirement):
    # Store original requirement
    context.original_requirement = requirement
    context.history = []
    
    # Stage 1: Clarify
    clarifier_result = invoke_agent(
        agent=Clarifier,
        requirement=requirement,
        context=context
    )
    
    if clarifier_result.type == "questions":
        relay_to_human(clarifier_result.questions)
        wait_for_human_response()
        context.acceptance_criteria = human_response
    else:
        context.acceptance_criteria = clarifier_result.proposed_criteria
        confirm_with_human(context.acceptance_criteria)
    
    context.history.append({
        agent: "Clarifier",
        status: "passed",
        result: clarifier_result
    })
    
    # Stage 2: Develop
    developer_result = invoke_agent(
        agent=Developer,
        requirement=context.original_requirement,
        acceptance_criteria=context.acceptance_criteria,
        context=context
    )
    
    context.history.append({
        agent: "Developer",
        status: "passed" if developer_result.tests_pass else "failed",
        result: developer_result
    })
    
    # [Continue for each agent...]
    # [Include retry logic for each stage]
    # [Include approval gates]
    
    return final_result
```

---

## Part 3: Example Plugin - Feature Delivery

### Creating the Plugin File

Create: `plugins/feature-delivery-pipeline.md`

```yaml
---
name: Feature-Delivery-Pipeline
version: 1.0.0
description: Complete workflow from requirement to merged and documented feature
category: workflow
status: stable
authors: [Your Team]
tags: [feature-development, high-throughput, human-approved]

requirements:
  agents: [BrainBox, Clarifier, Developer, Reviewer, Test, Docs, Checklist]
  skills: [Code-Quality-Check, Test-Coverage-Report, Detect-Breaking-Changes, Generate-Changelog]

invocation:
  syntax: "/feature-delivery {requirement}"
  params:
    requirement:
      type: string
      description: Feature description
      required: true

configuration:
  strict_mode: true
  retry_limit: 3
  timeout_minutes: 60
  approval_gates:
    - stage: post_clarification
    - stage: post_checklist

execution_plan:
  stages:
    - name: Clarification
      agents: [Clarifier]
      gate: human_confirms_criteria
    - name: Implementation
      agents: [Developer]
    - name: Code Review
      agents: [Reviewer]
    - name: Testing
      agents: [Test]
    - name: Documentation
      agents: [Docs]
    - name: Verification
      agents: [Checklist]

---

# Plugin: Feature-Delivery-Pipeline

[Rest of plugin documentation...]
```

### Linking in BrainBox

Update `BrainBox.agent.md`:
```yaml
---
name: Brainbox
description: Orchestrator for multi-agent development workflow
orchestrates_plugins:
  - Feature-Delivery-Pipeline
  - Bug-Fix-Express
  - Documentation-Sync
---
```

---

## Part 4: Plugin Examples - Complete Set

### Plugin 1: Bug-Fix-Express

```yaml
---
name: Bug-Fix-Express
version: 1.0.0
description: Fast-track bug fix workflow - no docs update, auto-approve if low-risk
category: utility
tags: [bug-fix, fast-track, auto-approved]

configuration:
  auto_approve_threshold: low-risk
  timeout_minutes: 15
  
execution_plan:
  stages:
    - name: Implementation
      agents: [Developer]
    - name: Code Review
      agents: [Reviewer]
    - name: Testing
      agents: [Test]
    - name: Verification
      agents: [Checklist]
      skip_documentation: true
  
  total_estimated_time: "15 minutes"

---

# Plugin: Bug-Fix-Express

## When to Use

- Simple bug fixes
- Low-risk changes
- Quick iterations
- Hotfixes (non-critical)

## Differences from Feature-Delivery

| Feature | Feature-Delivery | Bug-Fix-Express |
|---------|------------------|-----------------|
| Clarification | Required | Skipped |
| Docs Update | Yes | No |
| Code Review | Full | Quick |
| Auto-Approve | No | Yes (if low-risk) |
| Time | 45 min | 15 min |

## Example: Fix Off-by-One Error

```
/bug-fix-express "Fix off-by-one error in pagination - should be `page - 1` not `page`"
```

Expected workflow:
1. Developer: Write fix + test
2. Reviewer: Approve (likely 1 iteration)
3. Test: Verify
4. Checklist: Confirm
5. Auto-merge (no human gate for low-risk)
```

### Plugin 2: Documentation-Sync

```yaml
---
name: Documentation-Sync
version: 1.0.0
description: Keep docs in sync with code after changes
category: utility
tags: [documentation, automated]

invocation:
  syntax: "/docs-sync"

---

# Plugin: Documentation-Sync

## What This Does

After a feature is merged, this plugin:
1. Detects what code changed
2. Finds docs that reference changed code
3. Updates docs automatically
4. Creates PR for review

## Workflow

```
Code Merged → Detect Changes → Find Related Docs 
  → Update Docs → Create PR → Human Review
```

## Example

```
/docs-sync

Plugin runs:
- Detected: src/api/auth.ts changed
- Found: docs/api/authentication.md references it
- Updated: docs with new auth flow
- Created: PR #234 "Update auth documentation"
```
```

### Plugin 3: Release-Pipeline

```yaml
---
name: Release-Pipeline
version: 1.0.0
description: Complete release workflow - changelog, version bump, deploy
category: workflow
tags: [release, deployment]

invocation:
  syntax: "/release {version}"
  params:
    version:
      type: string
      description: "Semantic version (e.g., 1.2.0)"
      required: true

execution_plan:
  stages:
    - name: Generate Release Notes
      agents: [Docs]
      skills: [Generate-Changelog, Generate-Release-Notes]
    - name: Bump Version
      agents: [Developer]
    - name: Deploy to Staging
      agents: [DeploymentAgent]
    - name: Run Integration Tests
      agents: [Test]
    - name: Approval Gate
      gate: human_approves_production_deploy
    - name: Deploy to Production
      agents: [DeploymentAgent]
    - name: Verification
      agents: [Checklist]
    - name: Announce
      agents: [Docs]

total_estimated_time: "30 minutes"

---

# Plugin: Release-Pipeline

[Complete plugin documentation...]
```

---

## Part 5: Plugin Configuration & Customization

### Configuration Levels

#### Level 1: Default Configuration
```yaml
# plugins/feature-delivery-pipeline.md
configuration:
  strict_mode: true
  retry_limit: 3
```

#### Level 2: Team-Specific Configuration
```yaml
# config/team-config.yaml
plugins:
  feature-delivery-pipeline:
    retry_limit: 5  # Override: allow more retries
    timeout_minutes: 90  # Override: allow more time
    approval_gates:
      - post_checklist
      - pre_merge  # Add extra gate
```

#### Level 3: Invocation-Time Configuration
```
/feature-delivery \
  --requirement "Add OAuth" \
  --auto-approve low-risk \
  --timeout 120 \
  --skip-gate "pre-merge"
```

### Conditional Execution

Plugins can have conditional stages:

```yaml
execution_plan:
  stages:
    - name: Implementation
      agents: [Developer]
    
    - name: Security Scan  # Conditional
      agents: [SecurityAgent]
      condition: "change_affects_auth OR change_affects_data"
    
    - name: Performance Test  # Conditional
      agents: [PerformanceAgent]
      condition: "files_include('src/core/') OR files_include('src/db/')"
    
    - name: Code Review
      agents: [Reviewer]
    
    - name: Testing
      agents: [Test]
```

---

## Part 6: Best Practices for Plugins

### 1. **Clear Entry/Exit Criteria**

```yaml
preconditions:
  - Requirement is written in natural language
  - Repository is GitHub
  - CI/CD is configured

postconditions:
  - Code is merged to main
  - Documentation is updated
  - PR is closed
  - Team is notified
```

### 2. **Explicit Approval Gates**

Never auto-merge production code. Always require:
```yaml
approval_gates:
  - stage: post_checklist
    type: human_review
    auto_approve_conditions: []  # No auto-approve for main
```

### 3. **Timeouts & Escalation**

```yaml
configuration:
  timeout_minutes: 60
  escalation_action: "notify_human_with_context"
  escalation_condition: "timeout OR retry_limit_exceeded"
```

### 4. **Rollback Planning**

Document rollback for each plugin:
```markdown
## If Something Goes Wrong

1. **Code merged but bug found**: 
   - Revert commit
   - Create new issue
   - Use Bug-Fix-Express to fix
   
2. **Docs out of sync**:
   - Run Documentation-Sync plugin
   
3. **Test failed post-merge**:
   - CI should catch this, but if not:
   - Revert immediately
   - Investigate root cause
```

### 5. **Observability & Logging**

```yaml
monitoring:
  metrics_tracked:
    - total_duration
    - stage_durations
    - retry_count
    - approval_delays
    - success_rate
  logs:
    - BrainBox decisions
    - Agent outputs
    - Skill results
    - Human approvals
  retention: 90 days
```

---

## Part 7: Testing Your Plugin

### Test Case 1: Happy Path
```
Given: Clear, complete requirement
When: /feature-delivery invoked
Then: All stages pass in order
And: Checklist confirms
And: Human approves
And: Code merges successfully
```

### Test Case 2: Retry Loop
```
Given: Developer's code has quality issues
When: /feature-delivery invoked
Then: Reviewer finds issues
And: Developer notified
And: Developer fixes issues
And: Reviewer re-checks
And: Process continues
```

### Test Case 3: Escalation
```
Given: Developer fails after 3 retries
When: /feature-delivery orchestrating
Then: BrainBox escalates
And: Human gets detailed report
And: Human can manually intervene
```

### Test Case 4: Timeout
```
Given: Developer stage takes > 60 min
When: Plugin executing
Then: Timeout triggered
And: Human notified with context
```

---

## Part 8: Monitoring & Observability Dashboard

Create: `monitoring/plugin-dashboard.md`

```markdown
# Plugin Monitoring Dashboard

## Real-Time Metrics

- Current plugins running: 3
- Total executions today: 24
- Success rate: 95%
- Average duration: 38 min

## By Plugin

| Plugin | Count | Success % | Avg Duration | Last Run |
|--------|-------|-----------|--------------|----------|
| Feature-Delivery-Pipeline | 12 | 92% | 42 min | 2 min ago |
| Bug-Fix-Express | 10 | 100% | 14 min | 5 min ago |
| Documentation-Sync | 2 | 100% | 8 min | 30 min ago |

## Failure Analysis (Last 7 Days)

- Test stage: 2 failures (both resolved)
- Reviewer stage: 1 failure (merge conflict)
- Docs stage: 0 failures

## Retry Patterns

- Developer stage: 1.2 avg retries (expected)
- Reviewer stage: 0.3 avg retries (good)
- Test stage: 0.8 avg retries (expected)

## Approval Times

- Clarification gate: avg 5 min wait
- Final gate: avg 8 min wait
- Total approval time: avg 13 min of 45 min workflow

## Recommendations

- Consider auto-approving low-risk features (would save 8 min)
- Test stage has acceptable retry rate
- Documentation stage is consistently fast
```

---

## Part 9: Plugin Versioning & Evolution

### Version Strategy

```yaml
version: 1.0.0
changelog: |
  ## 1.0.0 (2024-01-15)
  - Initial stable release
  
  ## 0.5.0 (2024-01-10)
  - Beta: Conditional security scan
  - Beta: Performance testing optional
  
  ## 0.1.0 (2024-01-01)
  - Experimental: MVP workflow
```

### Breaking Changes

```yaml
# Migration from 1.x → 2.0

## What Changed
- approval_gates structure changed
- timeout renamed to timeout_minutes
- retry_limit now per-agent

## Required Agent Updates
- BrainBox: update orchestration logic
- All agents: update plugin integration

## Migration Steps
1. Update plugin YAML files
2. Test with feature flag
3. Gradual rollout
4. Document for users
```

---

## Part 10: Your Plugin Roadmap

### Q1: Foundation
- [ ] Feature-Delivery-Pipeline
- [ ] Bug-Fix-Express
- [ ] Documentation-Sync

### Q2: Expansion
- [ ] Release-Pipeline
- [ ] Security-Review-Pipeline
- [ ] Performance-Testing-Pipeline

### Q3: Optimization
- [ ] Conditional stages
- [ ] Parallel execution where possible
- [ ] Analytics dashboard

### Q4: Mastery
- [ ] Domain-specific plugins (Backend, Frontend, DevOps)
- [ ] Self-improving system
- [ ] Team customizations

---

## Quick Reference: Plugin Checklist

- [ ] Name is descriptive and actionable
- [ ] Clear description of what it does
- [ ] Prerequisites documented
- [ ] Invocation syntax defined with examples
- [ ] Execution stages in correct order
- [ ] Approval gates placed correctly
- [ ] Timeout and escalation configured
- [ ] Success/failure criteria defined
- [ ] Documentation complete with examples
- [ ] Test cases written
- [ ] Monitoring setup
- [ ] Rollback procedure documented
- [ ] Version bumped appropriately
- [ ] Linked in BrainBox agent

---

## Next Steps

1. **Create first plugin**: Feature-Delivery-Pipeline
2. **Test thoroughly**: Walk through entire workflow
3. **Document learnings**: Update plugin based on real usage
4. **Expand**: Create 2-3 more plugins for different workflows
5. **Optimize**: Add conditional stages and parallelization

Good luck! 🚀
