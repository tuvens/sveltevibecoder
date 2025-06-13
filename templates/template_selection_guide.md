# Template Selection Guide

> Quick decision framework for component templates

## Complexity Score (30 seconds)

| Factor | Points | Factor | Points |
|--------|--------|--------|--------|
| Props | 1 each | Child components | 2 each |
| State variables | 2 each | API calls | 3 each |
| Event handlers | 1 each | Conditionals | 1 each |

## Template Selection

| Score | Template | Context Cost |
|-------|----------|--------------|
| 0-8 | `templates/component-basic.svelte` | 40 tokens |
| 9-20 | `templates/component-advanced.svelte` | 60 tokens |
| 21+ | Escalate to senior agent | N/A |

## Examples

**Basic (0-8)**: Button, Avatar, Badge, LoadingSpinner
**Advanced (9-20)**: UserCard, SearchInput, Modal, DataTable  
**Escalate (21+)**: Dashboard, CompleteForm, PageLayout

## Protocol

1. Load this guide (15 tokens)
2. Calculate score
3. Load ONLY selected template
4. Never load multiple templates

**Uncertain?** Start with Basic, upgrade if needed.
2. **Switch templates if score changes category**
3. **Notify senior agent if escalation needed**
4. **Update task acknowledgment with new approach**

This guide enables efficient template selection without loading unnecessary context.
