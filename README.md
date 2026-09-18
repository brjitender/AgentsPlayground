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
