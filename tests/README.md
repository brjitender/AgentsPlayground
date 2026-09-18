# Tests Directory

Test specifications and strategies for agents, skills, and plugins.

## Test Structure

- `agent-tests.md` - Agent test cases
- `skill-tests.md` - Skill test cases
- `plugin-tests.md` - Plugin test cases

## Testing Strategy

Each skill/plugin should have:

1. **Happy Path Tests** (2-3 cases)
   - Normal operation with valid input
   - Expected output verification

2. **Error Path Tests** (2-3 cases)
   - Error scenarios
   - Error handling verification

3. **Edge Case Tests** (2-3 cases)
   - Boundary conditions
   - Unusual but valid inputs

4. **Integration Tests**
   - Skill + Tool interaction
   - Agent + Skill interaction
   - Plugin + Agent interaction

## Test Quality Standards

- Each skill: 5-8 test cases minimum
- Each plugin: 10-15 test cases minimum
- All error scenarios covered
- Edge cases documented
- Clear setup and verification

---

See docs/SKILL_DEVELOPMENT_GUIDE.md for testing details.

