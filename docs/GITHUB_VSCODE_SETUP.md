# 💻 GitHub + VS Code Implementation Guide

## Your Project Structure (Recommended)

```
AgentsPlayground/
├── .github/
│   ├── workflows/
│   │   ├── validate-agents.yml
│   │   ├── validate-skills.yml
│   │   ├── validate-plugins.yml
│   │   └── test-execution.yml
│   └── CODEOWNERS
│
├── agents/
│   ├── BrainBox.agent.md
│   ├── Clarifier.agent.md
│   ├── Developer.agent.md
│   ├── Reviewer.agent.md
│   ├── Test.agent.md
│   ├── Docs.agent.md
│   ├── Checklist.agent.md
│   └── README.md  # Agent index
│
├── skills/
│   ├── code-quality-check.md
│   ├── test-coverage-report.md
│   ├── generate-changelog.md
│   ├── detect-breaking-changes.md
│   ├── security-scan.md
│   ├── extract-api-changes.md
│   └── README.md  # Skills index
│
├── plugins/
│   ├── feature-delivery-pipeline.md
│   ├── bug-fix-express.md
│   ├── documentation-sync.md
│   ├── release-pipeline.md
│   └── README.md  # Plugins index
│
├── examples/
│   ├── skill-usage-examples.md
│   ├── plugin-invocation-examples.md
│   ├── agent-execution-logs.md
│   └── error-recovery-examples.md
│
├── tests/
│   ├── agent-tests.md
│   ├── skill-tests.md
│   ├── plugin-tests.md
│   └── integration-tests.md
│
├── docs/
│   ├── ARCHITECTURE.md
│   ├── GLOSSARY.md
│   ├── CONTRIBUTING.md
│   ├── TROUBLESHOOTING.md
│   └── FAQ.md
│
├── config/
│   ├── agents-config.yaml
│   ├── skills-config.yaml
│   ├── plugins-config.yaml
│   └── team-overrides.yaml
│
├── monitoring/
│   ├── plugin-dashboard.md
│   ├── agent-metrics.md
│   └── skill-performance.md
│
├── .gitignore
├── README.md
├── ROADMAP.md
└── CONTRIBUTING.md
```

---

## Part 1: Initial Setup (Day 1)

### Step 1: Create GitHub Repository

```bash
# Create repo on GitHub.com (if not already done)
gh repo create AgentsPlayground --public

# Clone locally
git clone https://github.com/YourUsername/AgentsPlayground.git
cd AgentsPlayground

# Set up Git flow
git config --local include.path ../.gitconfig
```

### Step 2: Create Directory Structure

```bash
mkdir -p agents skills plugins examples tests docs config monitoring

# Add .gitignore
cat > .gitignore << 'EOF'
# Logs
*.log
logs/
*.pid
*.seed

# Dependencies
node_modules/
.venv/
venv/
__pycache__/

# IDE
.vscode/settings.local.json
.idea/
*.swp
*.swo

# Environment
.env.local
.env.*.local
secrets/

# Generated files
/output/
/temp/
coverage/

# OS
.DS_Store
Thumbs.db
EOF

git add .gitignore
git commit -m "chore: add .gitignore"
```

### Step 3: Create README Structure

```bash
cat > README.md << 'EOF'
# 🤖 AgentsPlayground

Multi-agent orchestration system for automated software development workflows.

## Quick Start

```bash
# Invoke Feature-Delivery Pipeline
/feature-delivery "Add user authentication"

# Invoke Bug-Fix Express
/bug-fix-express "Fix login timeout"

# Invoke Documentation Sync
/docs-sync
```

## Architecture

- **Agents**: Specialized reasoning engines (Clarifier, Developer, Reviewer, Test, Docs, Checklist)
- **Skills**: Reusable task modules (Code-Quality-Check, Test-Coverage-Report, etc.)
- **Plugins**: Orchestrated workflows (Feature-Delivery-Pipeline, Bug-Fix-Express, etc.)
- **Tools**: Low-level capabilities (edit, search, run, runTests, read)

See [ARCHITECTURE.md](docs/ARCHITECTURE.md) for details.

## Documentation

- [Roadmap](ROADMAP.md) - Learning path and phases
- [Agents](agents/README.md) - Agent specifications
- [Skills](skills/README.md) - Available skills
- [Plugins](plugins/README.md) - Available plugins
- [Contributing](CONTRIBUTING.md) - How to contribute

## Status

Phase 1: Foundation ✅  
Phase 2: Skills Integration 🔄  
Phase 3: Plugin Development 🔜  

---

Made with ❤️ by your team
EOF

git add README.md
git commit -m "docs: add main README"
```

### Step 4: Copy Agent Files

```bash
# Move your agent files to the agents directory
cp /mnt/user-data/uploads/*_agent.md agents/

# Create agents index
cat > agents/README.md << 'EOF'
# 🤖 Agents

Specialized reasoning engines in AgentsPlayground.

## Core Agents

- **[BrainBox](BrainBox.agent.md)** - Orchestrator
- **[Clarifier](clarifier.agent.md)** - Requirement clarification
- **[Developer](developer.agent.md)** - Implementation
- **[Reviewer](reviewer.agent.md)** - Code quality review
- **[Test](test.agent.md)** - Testing & validation
- **[Docs](docs.agent.md)** - Documentation
- **[Checklist](checklist.agent.md)** - Final verification

## How Agents Work Together

1. **Clarifier** analyzes raw requirement → questions or acceptance criteria
2. **Developer** implements based on criteria
3. **Reviewer** checks code quality
4. **Test** validates against requirements
5. **Docs** keeps documentation in sync
6. **Checklist** verifies everything is complete
7. **BrainBox** orchestrates the entire workflow

## Creating New Agents

See [CONTRIBUTING.md](../CONTRIBUTING.md#creating-agents)
EOF

git add agents/
git commit -m "chore: add agent definitions"
```

### Step 5: Create GitHub Workflows

```bash
cat > .github/workflows/validate-agents.yml << 'EOF'
name: Validate Agents

on:
  pull_request:
    paths:
      - 'agents/**'
  push:
    branches: [main]
    paths:
      - 'agents/**'

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Check agent structure
        run: |
          for file in agents/*.agent.md; do
            echo "Validating $file..."
            # Check for required fields
            grep -q "^name:" "$file" || exit 1
            grep -q "^description:" "$file" || exit 1
            grep -q "^tools:" "$file" || exit 1
            grep -q "^---" "$file" || exit 1
          done
          
      - name: Lint markdown
        uses: nosborn/github-action-markdown-cli@v3.3.0
        with:
          files: agents

EOF

git add .github/workflows/validate-agents.yml
git commit -m "ci: add agent validation workflow"
```

---

## Part 2: VS Code Configuration

### Step 1: Install Extensions

In VS Code, install these extensions:

1. **Markdown All in One** (Yu Zhang)
   - Better markdown support
   - Syntax highlighting

2. **GitHub Copilot** (OpenAI)
   - AI-assisted coding
   - Already mentioned you have this ✅

3. **GitLens** (GitKraken)
   - Git history tracking
   - Blame annotations

4. **YAML** (Red Hat)
   - YAML validation for config files

5. **Prettier** (Prettier)
   - Code formatter consistency

6. **Error Lens** (Alexander)
   - Inline error messages

### Step 2: Configure VS Code

Create: `.vscode/settings.json`

```json
{
  // Editor
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.wordWrap": "on",
  "editor.rulers": [80, 120],
  
  // Markdown
  "markdown.preview.fontSize": 14,
  "[markdown]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true,
    "editor.wordWrap": "on"
  },
  
  // YAML
  "[yaml]": {
    "editor.insertSpaces": true,
    "editor.tabSize": 2,
    "editor.autoIndent": "keep"
  },
  
  // GitLens
  "gitlens.hovers.currentLine.enabled": true,
  "gitlens.blame.ignoreWhitespace": true,
  
  // Copilot
  "github.copilot.enable": {
    "*": true,
    "yaml": false,  // Less useful for YAML
    "markdown": false  // Keep markdown human-written
  },
  
  // Files
  "files.exclude": {
    "**/*.log": true,
    "**/node_modules": true,
    "**/__pycache__": true
  },
  
  "files.watcherExclude": {
    "**/.git/objects/**": true,
    "**/node_modules/**": true
  }
}
```

```bash
git add .vscode/settings.json
git commit -m "chore: configure VS Code"
```

### Step 3: Create VS Code Snippets

Create: `.vscode/agent.code-snippets`

```json
{
  "Agent Template": {
    "prefix": "agent",
    "body": [
      "---",
      "name: ${1:AgentName}",
      "description: ${2:Description of what this agent does}",
      "tools: ['${3:tool1}', '${4:tool2}']",
      "agents: ${5:['OtherAgent1']}",
      "model: ['Claude Opus 4.5', 'GPT-5.2']",
      "---",
      "",
      "# ${1:AgentName}",
      "",
      "## Job",
      "",
      "${0:Description of the agent's responsibilities}",
      "",
      "## Rules",
      "",
      "- ${0:Rule 1}",
      "- Rule 2"
    ],
    "description": "Create new agent definition"
  },
  
  "Skill Template": {
    "prefix": "skill",
    "body": [
      "---",
      "name: ${1:SkillName}",
      "version: 1.0.0",
      "description: ${2:What this skill does}",
      "category: ${3:development}",
      "complexity: ${4:medium}",
      "time_estimate: \"${5:15 min}\"",
      "tools_required: ['${6:tool1}', '${7:tool2}']",
      "agents_who_call_this: [${8:Developer}]",
      "---",
      "",
      "# Skill: ${1:SkillName}",
      "",
      "## What This Skill Does",
      "",
      "${0:Detailed description}",
      "",
      "## How It Works",
      "",
      "1. Step 1",
      "2. Step 2",
      "3. Step 3"
    ],
    "description": "Create new skill definition"
  }
}
```

---

## Part 3: GitHub Workflow Setup

### Step 1: Create Milestones

Go to GitHub > Issues > Milestones > New Milestone

```
Phase 1: Foundation (Week 1-2)
├── Skill: Code-Quality-Check
├── Skill: Test-Coverage-Report
├── Skill: Generate-Changelog
├── Skill: Detect-Breaking-Changes
└── Skill: Security-Scan

Phase 2: Integration (Week 3-4)
├── Refactor Developer agent for skills
├── Refactor Reviewer agent for skills
├── Refactor Docs agent for skills
└── Create Skills Index

Phase 3: Plugins (Week 5-6)
├── Plugin: Feature-Delivery-Pipeline
├── Plugin: Bug-Fix-Express
└── Plugin: Documentation-Sync

Phase 4: Advanced (Week 7-8)
├── Conditional skill invocation
├── Plugin composition
└── Observability dashboard
```

### Step 2: Create Issue Templates

Create: `.github/ISSUE_TEMPLATE/skill-creation.md`

```markdown
---
name: Create New Skill
about: Template for creating a new skill
title: "Skill: [Skill Name]"
labels: skill, phase-1
assignees: ''
---

## Skill Purpose
[What problem does this skill solve?]

## Who Uses It
[Which agents will call this skill?]

## Inputs
- Input 1: type, required/optional
- Input 2: type, required/optional

## Expected Output
[Describe the output format]

## Edge Cases
- Edge case 1
- Edge case 2

## Acceptance Criteria
- [ ] Skill definition written in YAML + Markdown
- [ ] Step-by-step process documented
- [ ] Input/output examples provided
- [ ] 3+ test cases written
- [ ] Linked from relevant agents
- [ ] Added to skills index
```

Create: `.github/ISSUE_TEMPLATE/plugin-creation.md`

```markdown
---
name: Create New Plugin
about: Template for creating a new plugin
title: "Plugin: [Plugin Name]"
labels: plugin, phase-3
assignees: ''
---

## Plugin Purpose
[What workflow does this orchestrate?]

## Agents Involved
- Agent 1 (why)
- Agent 2 (why)

## Skills Required
- Skill 1
- Skill 2

## Execution Stages
1. Stage 1 (description)
2. Stage 2 (description)
3. Stage 3 (description)

## Approval Gates
- Gate 1: [when?]
- Gate 2: [when?]

## Success Criteria
- [ ] Plugin orchestrates agents in correct order
- [ ] Approval gates work correctly
- [ ] Handles retries properly
- [ ] Documentation complete
- [ ] Tested end-to-end
```

### Step 3: Create Pull Request Template

Create: `.github/pull_request_template.md`

```markdown
## Description
[What does this PR do?]

## Related Issue
Fixes #[issue-number]
Relates to [milestone]

## Type of Change
- [ ] Skill creation
- [ ] Skill modification
- [ ] Agent update
- [ ] Plugin creation
- [ ] Documentation
- [ ] Other

## Checklist

### For Skill PRs
- [ ] YAML frontmatter complete
- [ ] Step-by-step process documented
- [ ] Input/output examples provided
- [ ] Edge cases documented
- [ ] 3+ test cases included
- [ ] Linked from agent description

### For Plugin PRs
- [ ] YAML frontmatter complete
- [ ] Execution flow documented
- [ ] Approval gates configured
- [ ] Examples provided
- [ ] Tested with sample requirement
- [ ] Documentation updated

### For All PRs
- [ ] Markdown is properly formatted
- [ ] Links are working
- [ ] No typos or grammar errors
- [ ] Related documentation updated

## Testing
[How was this tested?]

## Deployment Notes
[Any special deployment considerations?]
```

---

## Part 4: Daily Workflow with GitHub + VS Code

### Creating a New Skill (Workflow)

```bash
# 1. Create feature branch
git checkout -b feature/skill-code-quality-check

# 2. Create skill file in VS Code
# Use snippet: type "skill" + tab
# File: skills/code-quality-check.md

# 3. Write skill definition

# 4. Create corresponding test file
cat > tests/skill-code-quality-check-test.md << 'EOF'
# Tests for Code-Quality-Check Skill

## Test 1: Perfect Code
- Input: [...✓ good code]
- Expected: violations=[], passed=true
- Status: ✅ PASS

[More tests...]
EOF

# 5. Update skills index
# Edit: skills/README.md
# Add: - **[Code-Quality-Check](code-quality-check.md)** - Automated linting

# 6. Link from agent
# Edit: agents/reviewer.agent.md
# Add: skills_this_calls: [Code-Quality-Check]

# 7. Commit and push
git add skills/ tests/ agents/
git commit -m "feat: add Code-Quality-Check skill"
git push origin feature/skill-code-quality-check

# 8. Create Pull Request on GitHub
# Title: "Skill: Code-Quality-Check"
# Description: Use PR template
# Assignees: Yourself for now
# Milestone: Phase 1: Foundation
```

### Creating a New Plugin (Workflow)

```bash
# 1. Create feature branch
git checkout -b feature/plugin-feature-delivery

# 2. Create plugin file
# Use snippet: type "plugin" + tab
# File: plugins/feature-delivery-pipeline.md

# 3. Link related skills and agents

# 4. Create example usage
cat > examples/plugin-feature-delivery-example.md << 'EOF'
# Feature-Delivery-Pipeline Example

## Example: Add OAuth2 Authentication

```
/feature-delivery "Add OAuth2 authentication with GitHub and Google"
```

[Expected workflow...]
EOF

# 5. Write test cases
cat > tests/plugin-feature-delivery-test.md << 'EOF'
# Tests for Feature-Delivery-Pipeline

## Test: Happy Path
- Requirement: Add feature
- Expected: All stages pass → merge
- Status: ✅ PASS

[More tests...]
EOF

# 6. Commit and push
git add plugins/ examples/ tests/
git commit -m "feat: add Feature-Delivery-Pipeline plugin"
git push origin feature/plugin-feature-delivery

# 7. Create Pull Request
```

---

## Part 5: Collaboration with Copilot

### Using Copilot Effectively

#### For Documentation Writing
```
You: [Write skill description]
Copilot: Fills in edge cases, examples, test cases
Your: Review and refine
```

#### For Test Case Generation
```
You: [Skill name and purpose]
Copilot: Generates test cases
You: Review, adjust, confirm
```

#### For Example Creation
```
You: [Plugin invocation syntax]
Copilot: Generates workflow example
You: Review, adjust, confirm
```

**Pro Tip**: Turn off Copilot for YAML and Markdown to keep those human-written and high-quality.

### Copilot Prompting Strategy

```markdown
# When asking Copilot to help:

❌ "Generate a skill"
✅ "Create a skill definition for automated code linting with this structure:
   - YAML frontmatter with name, description, inputs, outputs
   - Step-by-step process
   - 3 test cases
   Here's an example: [paste Code-Quality-Check skill]"

❌ "Add test cases"
✅ "Add 2 more test cases for this skill:
   - Happy path where [condition]
   - Edge case where [condition]
   Follow this format: [show example]"
```

---

## Part 6: GitHub Projects & Tracking

### Create GitHub Project (New)

1. Go to **Projects** tab
2. **New project** (Table view)
3. Add fields:
   - Title
   - Phase (Phase 1, Phase 2, etc.)
   - Status (Not Started, In Progress, Review, Done)
   - Assignee
   - Due Date
   - Type (Skill, Plugin, Agent, Docs)

### Populate Initial Tasks

Add issues for Phase 1:
```
- Skill: Code-Quality-Check (Phase 1, Assigned to You)
- Skill: Test-Coverage-Report (Phase 1)
- Skill: Generate-Changelog (Phase 1)
- Skill: Detect-Breaking-Changes (Phase 1)
- Skill: Security-Scan (Phase 1)
- Refactor: Developer agent (Phase 2)
- Refactor: Reviewer agent (Phase 2)
```

---

## Part 7: Commit Message Convention

Use conventional commits:

```bash
# Skill creation
git commit -m "feat(skill): add code-quality-check"

# Skill update
git commit -m "refactor(skill): improve code-quality-check performance"

# Plugin creation
git commit -m "feat(plugin): add feature-delivery-pipeline"

# Agent refactor
git commit -m "refactor(agent): integrate skills into reviewer"

# Documentation
git commit -m "docs: update skills index"

# Bug fix
git commit -m "fix(skill): handle missing config gracefully"

# Tests
git commit -m "test: add test cases for code-quality-check"
```

Then in your PR description, link issues:
```
Fixes #5
Relates to #3
Part of milestone: Phase 1 Foundation
```

---

## Part 8: Quick Reference - Your Daily Commands

```bash
# Start work on a skill
git checkout -b feature/skill-{skillname}

# Write skill file in VS Code
# Then:
git add skills/ tests/ agents/
git status  # Review

# Commit
git commit -m "feat(skill): add {skillname}"
git push origin feature/skill-{skillname}

# Create PR on GitHub (or use CLI)
gh pr create --title "Skill: {SkillName}" \
  --body "See description in PR template" \
  --milestone "Phase 1: Foundation" \
  --assignee @me

# After merge, sync main
git checkout main
git pull origin main

# Start next skill
git checkout -b feature/skill-next-skillname
```

---

## Part 9: GitHub Actions - Auto-Validation

Create: `.github/workflows/validate-skills.yml`

```yaml
name: Validate Skills

on:
  pull_request:
    paths:
      - 'skills/**'
  push:
    branches: [main]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Validate YAML frontmatter
        run: |
          for file in skills/*.md; do
            echo "Validating $file..."
            # Check required fields
            grep -q "^name:" "$file" || (echo "Missing name in $file" && exit 1)
            grep -q "^version:" "$file" || (echo "Missing version in $file" && exit 1)
            grep -q "^description:" "$file" || (echo "Missing description in $file" && exit 1)
            grep -q "^tools_required:" "$file" || (echo "Missing tools_required in $file" && exit 1)
          done
      
      - name: Check documentation completeness
        run: |
          for file in skills/*.md; do
            echo "Checking docs in $file..."
            grep -q "## What" "$file" || (echo "Missing 'What' section" && exit 1)
            grep -q "## How It Works" "$file" || (echo "Missing 'How It Works' section" && exit 1)
            grep -q "## Testing Strategy" "$file" || (echo "Missing 'Testing Strategy' section" && exit 1)
          done
      
      - name: Lint markdown
        uses: nosborn/github-action-markdown-cli@v3.3.0
        with:
          files: skills
          config_file: .markdownlintrc
```

---

## Summary

You now have:

1. ✅ **Project Structure** - Organized directories for agents, skills, plugins
2. ✅ **VS Code Setup** - Extensions, settings, snippets
3. ✅ **GitHub Workflow** - Issue templates, PR templates, workflows
4. ✅ **Documentation** - Comprehensive guides for skills and plugins
5. ✅ **Tracking** - GitHub Projects for progress tracking
6. ✅ **CI/CD** - Automated validation on PRs
7. ✅ **Collaboration** - Copilot integration

## Next Steps (Today)

- [ ] Push all files to GitHub
- [ ] Configure GitHub Projects
- [ ] Create Phase 1 issues
- [ ] Start first skill (Code-Quality-Check)
- [ ] Get familiar with the workflow

Good luck! 🚀
