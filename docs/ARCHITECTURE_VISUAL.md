# 🏗️ Architecture Visual Guide

## The Complete System Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          USER INVOCATION                                │
│                  /feature-delivery "Requirement text"                   │
└────────────────────────────────────┬────────────────────────────────────┘
                                     │
                                     ↓
┌─────────────────────────────────────────────────────────────────────────┐
│                      PLUGIN ORCHESTRATION LAYER                         │
│                    Feature-Delivery-Pipeline Plugin                     │
│                                                                         │
│  Handles: Routing, Retries, Approval Gates, Timeouts                   │
│  Coordinates: 7 agents in sequence with checks                         │
│  Manages: Context sharing, error recovery, escalation                  │
└────────────┬──────────────────────────────────────────────────────────┬─┘
             │                                                           │
    Sequential Orchestration (7 Stages)                    Error/Timeout
             │                                               Escalation
             ↓
┌─────────────────────────────────────────────────────────────────────────┐
│                         AGENT EXECUTION LAYER                           │
│                                                                         │
│  1. Clarifier Agent        Accepts: Requirement text                   │
│     ├─ Analyzes requirement                                            │
│     ├─ Asks clarifying questions                                       │
│     └─ Returns: Acceptance criteria (locked)                           │
│                                                                         │
│  2. Developer Agent        Accepts: Requirement + Criteria             │
│     ├─ Calls Skill: Generate-Unit-Tests                               │
│     ├─ Writes implementation                                           │
│     ├─ Calls Skill: Code-Quality-Check                                │
│     ├─ Runs tests                                                      │
│     └─ Returns: Changed files + test results                          │
│                                                                         │
│  3. Reviewer Agent         Accepts: Developer's changes               │
│     ├─ Calls Skill: Code-Quality-Check                                │
│     ├─ Calls Skill: Detect-Breaking-Changes                           │
│     ├─ Calls Skill: Security-Scan                                     │
│     └─ Returns: Issues (blocking vs should-fix)                      │
│                                                                         │
│  4. Test Agent             Accepts: Implementation + Criteria          │
│     ├─ Calls Skill: Test-Coverage-Report                              │
│     ├─ Calls Skill: Requirement-to-Tests                              │
│     └─ Returns: Coverage %, gaps, failed tests                        │
│                                                                         │
│  5. Docs Agent             Accepts: Implementation summary             │
│     ├─ Calls Skill: Extract-API-Changes                               │
│     ├─ Calls Skill: Generate-Changelog                                │
│     ├─ Calls Skill: Generate-README-Update                            │
│     └─ Returns: Updated docs files                                    │
│                                                                         │
│  6. Checklist Agent        Accepts: All criteria + all changes        │
│     ├─ Verifies each criterion met                                    │
│     ├─ Checks tests exist and pass                                    │
│     └─ Returns: ✅ Complete or ❌ gaps to fix                        │
│                                                                         │
│  7. (Deployment Agent)     Optional for Release-Pipeline              │
│     ├─ Deploys to staging                                             │
│     ├─ Runs integration tests                                         │
│     └─ Deploys to production                                          │
│                                                                         │
└─────────────┬────────────────────────────────────────────────────────┬─┘
              │                                                         │
       Each agent calls skills    ←Error/Gaps→    Retry loop back
              │                                      to specific agent
              ↓
┌─────────────────────────────────────────────────────────────────────────┐
│                           SKILL LAYER                                   │
│                                                                         │
│  Skill: Code-Quality-Check         Skill: Test-Coverage-Report         │
│  ├─ Input: file paths              ├─ Input: test suite path          │
│  ├─ Process: run linter            ├─ Process: run coverage report    │
│  ├─ Output: violations list        ├─ Output: coverage %, gaps        │
│  └─ Called by: Developer, Reviewer └─ Called by: Developer, Test      │
│                                                                         │
│  Skill: Generate-Changelog         Skill: Detect-Breaking-Changes      │
│  ├─ Input: commit range            ├─ Input: code diff                │
│  ├─ Process: parse commits         ├─ Process: analyze signatures     │
│  ├─ Output: formatted changelog    ├─ Output: breaking changes list   │
│  └─ Called by: Docs                └─ Called by: Reviewer             │
│                                                                         │
│  Skill: Security-Scan              Skill: Extract-API-Changes         │
│  ├─ Input: code files              ├─ Input: implementation           │
│  ├─ Process: detect vulnerabilities├─ Process: find API changes       │
│  ├─ Output: security issues        ├─ Output: API diff                │
│  └─ Called by: Reviewer            └─ Called by: Docs                 │
│                                                                         │
│  Skill: Generate-README-Update     Skill: Requirement-to-Tests        │
│  ├─ Input: API changes             ├─ Input: acceptance criteria      │
│  ├─ Process: update README         ├─ Process: generate test stubs    │
│  ├─ Output: updated README         ├─ Output: test file               │
│  └─ Called by: Docs                └─ Called by: Developer, Test      │
│                                                                         │
│  + More skills as needed...                                            │
│                                                                         │
└─────────────┬────────────────────────────────────────────────────────┬─┘
              │                                                         │
       Each skill uses tools  ←─ Skill defines its tools ─→ Tools list
              │
              ↓
┌─────────────────────────────────────────────────────────────────────────┐
│                            TOOL LAYER                                   │
│                                                                         │
│  edit            read/file        runTests        run (bash)          │
│  search/codebase read/code        read/commit     search/usages       │
│                                                                         │
│  [All tools available to agents/skills via framework]                 │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Data Flow Example: Feature-Delivery-Pipeline

```
START: User invokes /feature-delivery "Add OAuth2"
  │
  ├─ BrainBox stores:
  │  ├─ original_requirement: "Add OAuth2"
  │  ├─ history: []
  │  ├─ acceptance_criteria: (not yet set)
  │  └─ retry_counters: {}
  │
  ├─→ Call Clarifier Agent
  │  │
  │  ├─ Receives: requirement = "Add OAuth2"
  │  │
  │  ├─ Clarifier asks:
  │  │  ├─ "Which OAuth providers?"
  │  │  ├─ "User database storage requirements?"
  │  │  └─ "Testing requirements?"
  │  │
  │  ├─ Human responds:
  │  │  ├─ "GitHub and Google"
  │  │  ├─ "New user_auth table"
  │  │  └─ "Unit tests for each provider"
  │  │
  │  └─ Returns:
  │     ├─ questions_asked: [3 questions]
  │     └─ acceptance_criteria: [4 specific criteria]
  │
  ├─ BrainBox stores:
  │  └─ acceptance_criteria = [locked criteria]
  │
  ├─→ Call Developer Agent
  │  │
  │  ├─ Receives:
  │  │  ├─ requirement: "Add OAuth2"
  │  │  ├─ acceptance_criteria: [criteria]
  │  │  └─ context: (shared history)
  │  │
  │  ├─ Developer writes code:
  │  │  ├─ Creates: src/auth/oauth.ts
  │  │  ├─ Creates: migrations/001_auth_table.sql
  │  │  └─ Creates: tests/auth.test.ts
  │  │
  │  ├─ Developer invokes Skill: Code-Quality-Check
  │  │  ├─ Input: ["src/auth/oauth.ts", "src/auth/index.ts"]
  │  │  └─ Output: violations: [1 warning], passed: true
  │  │
  │  ├─ Developer invokes Skill: Test-Coverage-Report
  │  │  ├─ Input: test_framework: "jest"
  │  │  └─ Output: coverage: 88%, untested_criteria: [], passed: true
  │  │
  │  └─ Developer returns:
  │     ├─ status: "passed"
  │     ├─ files_changed: [3 files]
  │     ├─ tests_added: 12
  │     └─ skill_results: [Quality pass, Coverage pass]
  │
  ├─ BrainBox stores:
  │  └─ history: [Clarifier: ✅, Developer: ✅]
  │
  ├─→ Call Reviewer Agent
  │  │
  │  ├─ Receives:
  │  │  ├─ developer_output: [changes summary]
  │  │  ├─ requirement: "Add OAuth2"
  │  │  └─ context: (shared history)
  │  │
  │  ├─ Reviewer invokes Skill: Code-Quality-Check
  │  │  └─ Output: violations: [], passed: true
  │  │
  │  ├─ Reviewer invokes Skill: Detect-Breaking-Changes
  │  │  ├─ Analyzes: API changes
  │  │  └─ Output: breaking_changes: [], passed: true
  │  │
  │  ├─ Reviewer invokes Skill: Security-Scan
  │  │  ├─ Checks: no hardcoded secrets, no injection risks
  │  │  └─ Output: vulnerabilities: [], passed: true
  │  │
  │  └─ Reviewer returns:
  │     ├─ status: "passed"
  │     ├─ blocking_issues: []
  │     └─ should_fix: []
  │
  ├─ BrainBox stores:
  │  └─ history: [..., Reviewer: ✅]
  │
  ├─→ Call Test Agent
  │  │
  │  ├─ Invokes Skill: Test-Coverage-Report
  │  ├─ Invokes Skill: Requirement-to-Tests
  │  └─ Output: all_tests_pass: true, coverage: 88%
  │
  ├─→ Call Docs Agent
  │  │
  │  ├─ Invokes Skill: Extract-API-Changes
  │  ├─ Invokes Skill: Generate-Changelog
  │  ├─ Invokes Skill: Generate-README-Update
  │  └─ Updates: API docs, CHANGELOG.md, README.md
  │
  ├─→ Call Checklist Agent
  │  │
  │  ├─ Verifies each acceptance criterion:
  │  │  ├─ ✅ GitHub OAuth provider implemented
  │  │  ├─ ✅ Google OAuth provider implemented
  │  │  ├─ ✅ user_auth table created
  │  │  ├─ ✅ Unit tests pass
  │  │  ├─ ✅ Documentation updated
  │  │  └─ ✅ Code quality checks pass
  │  │
  │  └─ Returns: status: "all_criteria_met"
  │
  ├─ BrainBox APPROVAL GATE:
  │  ├─ Sends to human: "All criteria met, approve merge?"
  │  └─ Human: "✅ Approve"
  │
  ├─→ Final Actions:
  │  ├─ Create commit with all changes
  │  ├─ Auto-merge to main
  │  ├─ Close related issues
  │  └─ Send team notification
  │
  └─ END: Feature delivered, tested, documented, merged

TOTAL TIME: ~45 minutes (automated + human gates)
WITHOUT AUTOMATION: ~4-8 hours (manual review cycles)
```

---

## Skill Dependency Graph

```
User Requirement
    │
    ├─→ Clarifier (no skills)
    │
    ├─→ Developer uses:
    │   ├─ Generate-Unit-Tests
    │   ├─ Code-Quality-Check
    │   └─ Test-Coverage-Report
    │
    ├─→ Reviewer uses:
    │   ├─ Code-Quality-Check  (reused ↑)
    │   ├─ Detect-Breaking-Changes
    │   └─ Security-Scan
    │
    ├─→ Test uses:
    │   ├─ Test-Coverage-Report  (reused ↑)
    │   └─ Requirement-to-Tests
    │
    ├─→ Docs uses:
    │   ├─ Extract-API-Changes
    │   ├─ Generate-Changelog
    │   └─ Generate-README-Update
    │
    └─→ Checklist (no skills)
        ├─ Verifies: All skills ran successfully
        └─ Cross-checks: All criteria met
```

---

## Plugin Stacking (Advanced)

```
User invokes: /release v1.2.0

Release-Pipeline Plugin
  │
  ├─ Stage 1: Generate Release Notes
  │  └─ Calls: Feature-Delivery-Pipeline  ← Sub-plugin!
  │     (for each feature shipped)
  │
  ├─ Stage 2: Bump Version
  │  └─ Developer agent
  │
  ├─ Stage 3: Deploy to Staging
  │  └─ Deployment agent
  │
  ├─ Stage 4: Run Integration Tests
  │  └─ Test agent
  │
  ├─ Stage 5: Deploy to Production
  │  └─ Deployment agent
  │
  └─ Stage 6: Announce Release
     └─ Docs agent

Result: Entire release automated!
```

---

## Retry Loop Visual

```
Developer Stage:
  │
  ├─ Write code
  ├─ Run tests
  ├─ Call skills
  │
  └─ Output → Reviewer Stage
              │
              ├─ Check code quality
              ├─ Detect breaking changes
              │
              └─ Blocking issues found? → YES
                                           │
                                           ├─→ Report issues to Developer
                                           │
                                           ├─ Developer fixes issues
                                           │
                                           ├─ Developer re-runs tests
                                           │
                                           ├─ Developer re-invokes skills
                                           │
                                           └─ Output → Reviewer Stage (again)
                                                       │
                                                       └─ Issues resolved?
                                                           │
                                                           ├─ NO → Loop again
                                                           └─ YES → Continue
```

---

## Configuration Precedence

```
Default (in skill/plugin definitions)
    ↓
Team Override (config/team-overrides.yaml)
    ↓
Invocation-Time (command line flags)
    ↓
Final Configuration Used
```

---

## Skills Reusability Matrix

```
                  Developer  Reviewer  Test  Docs  Checklist
Code-Quality        ✅        ✅              
Test-Coverage       ✅        ✅        ✅        
Generate-Changelog                            ✅    
Detect-Breaking              ✅              
Security-Scan               ✅              
Extract-API-Changes                          ✅    
Generate-README                              ✅    
Requirement-Tests  ✅                 ✅      
Generate-Unit      ✅                        

Legend:
✅ = This agent calls this skill
```

Skills are designed to be reused across multiple agents, reducing duplication!

---

## Your Building Journey

```
Week 1-2: Build Skills Foundation
Skills Layer ←── You are here
  ├─ Code-Quality-Check
  ├─ Test-Coverage-Report
  ├─ Generate-Changelog
  ├─ Detect-Breaking-Changes
  └─ Security-Scan

Week 3-4: Integrate Skills
Agent Layer Refactoring
  ├─ Developer now calls skills
  ├─ Reviewer now calls skills
  ├─ Test now calls skills
  └─ Docs now calls skills

Week 5-6: Build Plugins
Plugin Orchestration Layer ←── You'll be here next
  ├─ Feature-Delivery-Pipeline
  ├─ Bug-Fix-Express
  └─ Documentation-Sync

Week 7-8: Advanced Patterns
Optimization & Composition
  ├─ Conditional skill invocation
  ├─ Plugin chaining
  └─ Observability

Week 9+: Mastery
Domain-Specific Workflows
  ├─ Backend-Development-Pipeline
  ├─ Frontend-Development-Pipeline
  └─ DevOps-Release-Pipeline
```

---

## Summary

Your system has 4 layers:

1. **Tools** (Low-level capabilities)
   - edit, search, run, runTests, read

2. **Skills** (Reusable task modules)
   - Code-Quality-Check, Test-Coverage-Report, etc.

3. **Agents** (Specialized reasoning engines)
   - Clarifier, Developer, Reviewer, Test, Docs, Checklist

4. **Plugins** (Orchestrated workflows)
   - Feature-Delivery-Pipeline, Bug-Fix-Express, etc.

Each layer builds on the previous one, creating a powerful automation system.

**You're building the Skills layer right now. 🚀**
