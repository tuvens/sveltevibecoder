# Component Architecture Quick Reference

> 50-line decision guide for component structure

## Size Decision Tree

```
Component Assessment
├── Single purpose + < 50 lines → Small (Basic template)
├── Multiple concerns + 50-150 lines → Medium (Advanced template)  
├── Complex logic + > 150 lines → Large (Senior agent)
└── Unclear → Start small, escalate if needed
```

## Quick Decision Matrix

| Factor | Small | Medium | Large |
|--------|-------|--------|-------|
| **Props** | 1-3 | 4-6 | 7+ |
| **State** | 1-2 | 3-5 | 6+ |
| **Children** | 0-1 | 2-4 | 5+ |
| **Events** | 1-2 | 3-5 | 6+ |
| **API calls** | 0-1 | 2-3 | 4+ |

## Component Examples by Size

### Small Components (Basic Template)
**Examples**: Button, Avatar, Badge, Icon, LoadingSpinner
**Pattern**: Single responsibility, minimal state
**Template**: `templates/component-basic.svelte`

### Medium Components (Advanced Template)  
**Examples**: UserCard, SearchInput, Modal, DataTable
**Pattern**: Multiple concerns, managed state
**Template**: `templates/component-advanced.svelte`

### Large Components (Senior Agent)
**Examples**: Dashboard, PageLayout, CompleteForm
**Pattern**: Complex orchestration, architecture needed

## State Management by Size

| State Type | Small | Medium | Large |
|------------|-------|--------|-------|
| **Local** | `let loading = false` | Multiple variables | State machines |
| **Props** | `export let user` | Object props | Interface design |
| **Events** | `dispatch('click')` | Complex payloads | Event architecture |

## Common Mistakes

| Issue | Small | Medium | Fix |
|-------|-------|--------|-----|
| **Too many props** | > 3 | > 6 | Break into smaller components |
| **Mixed concerns** | Display + logic | Business + UI | Separate responsibilities |
| **Deep nesting** | > 2 levels | > 3 levels | Extract child components |

## 30-Second Assessment Questions

1. **Primary purpose?** Single = Small, Multiple = Medium+
2. **Props needed?** 1-3 = Small, 4-6 = Medium, 7+ = Large  
3. **State complexity?** Simple = Small, Managed = Medium, Complex = Large
4. **Integration needs?** Few = Small, Some = Medium, Many = Large

**Complexity Score**: Props + State + Events + Children + APIs
- **0-8**: Small (Basic template)
- **9-20**: Medium (Advanced template)
- **21+**: Large (Senior agent)
