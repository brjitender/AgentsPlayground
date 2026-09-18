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
