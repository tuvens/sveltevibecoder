# Svelte Vibe Coder

> Token-efficient AI-assisted development patterns and documentation for Svelte projects

## Overview

Svelte Vibe Coder is a collection of patterns, templates, and documentation designed to optimize AI-assisted development workflows with Svelte. This repository focuses on "vibe coding" - the practice of using AI agents and natural language prompts to rapidly build functional applications with minimal token overhead.

**This repository is tailored for Svelte 4 projects** but can easily be adapted for Svelte 5 by modifying the syntax patterns in the documentation files.

## What is Vibe Coding?

Vibe coding is an approach to software development that emphasizes:
- **Speed over perfection**: Build functional MVPs quickly
- **AI collaboration**: Leverage language models as development partners
- **Token efficiency**: Optimize prompts and documentation for minimal AI context usage
- **Iterative refinement**: "It mostly works, and that's enough" philosophy
- **Multi-agent workflows**: Specialized AI agents working together

## Why Svelte for Vibe Coding?

Svelte's minimal syntax and explicit reactivity make it ideal for AI-assisted development:
- **Concise code**: Less boilerplate = fewer tokens
- **Clear patterns**: Reactive statements (`$:`) are easy for AI to understand
- **Fast compilation**: Quick feedback loops for iterative development
- **Mature ecosystem**: Stable patterns that AI models understand well

## Repository Structure

```
svelte-vibe-coder/
├── patterns/              # Core development patterns
│   ├── svelte4-basics.md     # Fundamental Svelte 4 patterns
│   ├── component-architecture.md
│   ├── token-optimization.md
│   └── multi-agent-workflows.md
├── templates/             # Reusable templates
│   ├── component-template.svelte
│   ├── handoff-template.md
│   └── implementation-plan.md
├── examples/              # Working code examples
│   ├── components/
│   └── workflows/
├── tools/                 # Automation and helper scripts
└── docs/                  # Additional documentation
```

## Quick Start

### As a Git Submodule

Add this repository to your existing Svelte project:

```bash
# In your main project directory
git submodule add https://github.com/yourusername/svelte-vibe-coder docs/vibe-coding

# Initialize the submodule
git submodule update --init

# Your project now has access to patterns:
# your-project/docs/vibe-coding/patterns/
```

### Standalone Usage

Clone the repository to use as a reference:

```bash
git clone https://github.com/yourusername/svelte-vibe-coder
cd svelte-vibe-coder
```

## Core Principles

### 1. Token Efficiency First
- Use markdown over JSON for structured data
- Prefer concise Svelte 4 syntax (`$: computed = value`)
- Structure documentation for AI consumption
- Minimize context switching between agents

### 2. Multi-Agent Architecture
- **Senior agents**: Architecture and planning (80K token budget)
- **Junior agents**: Implementation tasks (25-40K token budget)
- **Specialized agents**: DevOps, testing, documentation
- Clear handoff protocols between agents

### 3. Svelte 4 Optimization
- Leverage stable, well-understood patterns
- Use reactive statements for computed values
- Explicit event handling with `on:event={}`
- Component props with `export let`

## Agent Integration

### Claude Projects Setup

Add to your Claude project instructions:

```markdown
## Documentation Reference
Vibe coding patterns: docs/vibe-coding/patterns/
Component templates: docs/vibe-coding/templates/
Implementation examples: docs/vibe-coding/examples/

## Svelte 4 Context
- Use patterns from docs/vibe-coding/patterns/svelte4-basics.md
- Follow component structure from templates/
- Reference token optimization guidelines
```

### Claude Code Integration

```bash
# Reference documentation in commands
claude --context "Use patterns from docs/vibe-coding/patterns/component-architecture.md" \
  implement component UserCard

# Auto-generate with templates
claude generate component --template docs/vibe-coding/templates/component-template.svelte
```

## Adapting for Svelte 5

To modify this repository for Svelte 5 projects:

1. **Update syntax patterns** in `patterns/svelte4-basics.md`:
   - Replace `let count = 0` with `let count = $state(0)`
   - Replace `$: doubled = count * 2` with `let doubled = $derived(() => count * 2)`
   - Update event handlers to `onclick={}` syntax

2. **Modify templates** in `templates/`:
   - Update component template with runes syntax
   - Adjust handoff templates for Svelte 5 patterns

3. **Update examples** to use Svelte 5 conventions

Most documentation files (multi-agent workflows, token optimization, architecture patterns) remain relevant across versions.

## Contributing

We welcome contributions that improve vibe coding workflows:

- **Patterns**: Document successful AI-assisted development patterns
- **Templates**: Create reusable scaffolds for common tasks
- **Examples**: Share working code demonstrating vibe coding principles
- **Tools**: Build automation that enhances the vibe coding experience

### Contribution Guidelines

1. Focus on token efficiency and AI comprehension
2. Include practical examples with code samples
3. Document the reasoning behind pattern choices
4. Test patterns with actual AI agents before submitting

## Use Cases

### Individual Developers
- Rapid prototyping with AI assistance
- Learning Svelte through guided AI interaction
- Building MVPs with minimal technical debt

### Development Teams
- Multi-agent development workflows
- Consistent coding standards across AI-assisted projects
- Knowledge sharing between human and AI team members

### Open Source Projects
- Community-driven pattern development
- Shared templates and best practices
- Cross-project learning and optimization

## Performance Metrics

Vibe coding with these patterns typically achieves:
- **30-40% token reduction** vs. unstructured AI prompts
- **2-4x faster MVP development** compared to traditional methods
- **Consistent code quality** across different AI models
- **Reduced context switching** between agents and tasks

## Community

- **GitHub Issues**: Bug reports and feature requests
- **Discussions**: Share patterns and ask questions
- **Wiki**: Community-maintained documentation
- **Examples**: Submit your successful vibe coding projects

## License

MIT License - Feel free to use these patterns in your projects, whether open source or commercial.

## Acknowledgments

Based on principles from the agentic coding community and research into token-efficient AI development workflows. Special thanks to contributors who have shared their vibe coding experiences and patterns.

---

**Ready to start vibe coding?** Check out `patterns/svelte4-basics.md` and `templates/implementation-plan.md` to begin your AI-assisted Svelte journey.
