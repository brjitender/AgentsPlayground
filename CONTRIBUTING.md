# Contributing to AgentsPlayground

Thank you for your interest in contributing! This guide will help you create quality skills and plugins.

## Getting Started

1. Clone the repository
2. Create a branch: `git checkout -b feature/skill-name`
3. Read the relevant guide:
   - **Skills**: `docs/SKILL_DEVELOPMENT_GUIDE.md`
   - **Plugins**: `docs/PLUGIN_DEVELOPMENT_GUIDE.md`

## Process

### For Skills

1. Copy `skills/TEMPLATE.md`
2. Fill in all sections completely
3. Write at least 5 test cases
4. Add usage examples
5. Commit: `git commit -m "feat(skill): add skill-name"`
6. Create PR with description

### For Plugins

1. Copy `plugins/TEMPLATE.md`
2. Define all workflow stages
3. Configure approval gates
4. Write examples
5. Document all error scenarios
6. Commit: `git commit -m "feat(plugin): add plugin-name"`
7. Create PR with description

## Code Style

- **Markdown**: Use proper heading hierarchy (# → ##)
- **YAML**: Use 2-space indentation
- **Examples**: Always include real, runnable examples
- **Documentation**: Clear and concise explanations

## Commit Messages

Format: `type(scope): subject`

Examples:
- `feat(skill): add code-quality-check skill`
- `docs(plugin): improve feature-delivery-pipeline docs`
- `fix(skill): handle edge case in test-coverage-report`

Types: feat, fix, docs, refactor, test, style

## Pull Request Checklist

- [ ] All [TODO] sections completed
- [ ] At least 3-5 test cases written
- [ ] Error scenarios documented
- [ ] Examples provided
- [ ] Documentation is clear
- [ ] No placeholder text remains
- [ ] Tested with Copilot
- [ ] Ready for peer review

## Skill Quality Standards

- **Completeness**: All sections filled in
- **Clarity**: Clear to someone unfamiliar with your work
- **Testability**: Can be tested in isolation
- **Reusability**: Useful to multiple agents/plugins
- **Documentation**: Examples and edge cases covered

## Plugin Quality Standards

- **Workflow**: Clear stage definitions
- **Gates**: Appropriate approval gates configured
- **Examples**: At least 2 real-world examples
- **Error handling**: All failure scenarios covered
- **Documentation**: Complete and understandable

## Review Process

1. Submit PR with description
2. Automated tests run (GitHub Actions)
3. Code review by maintainers
4. Address feedback
5. Approval and merge

## Question?

Check:
- `docs/FAQ.md` for common questions
- `docs/TROUBLESHOOTING.md` for problems
- Open a GitHub discussion

---

**Thank you for contributing!**

