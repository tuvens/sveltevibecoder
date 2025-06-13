# Template Selection Guide

> Decision framework for choosing component templates (NO TEMPLATES INCLUDED)

## 30-Second Complexity Assessment

### Quick Scoring System
Count the following and add points:
- **Props needed**: 1 point each
- **State variables**: 2 points each  
- **Event handlers**: 1 point each
- **Child components**: 2 points each
- **API calls**: 3 points each
- **Conditional branches**: 1 point each

### Template Selection Rules
| Score | Template | File Path | Context Cost |
|-------|----------|-----------|--------------|
| **0-8 points** | Basic | `templates/component-basic.svelte` | 25 tokens |
| **9-20 points** | Advanced | `templates/component-advanced.svelte` | 45 tokens |
| **21+ points** | Escalate | Senior agent planning required | N/A |

## Quick Examples

### Basic Template (0-8 points)
- **Button**: 2 props + 1 handler = 3 points → Basic
- **Avatar**: 2 props + 1 state = 4 points → Basic  
- **Badge**: 1 prop + 1 conditional = 2 points → Basic
- **LoadingSpinner**: 2 props = 2 points → Basic

### Advanced Template (9-20 points)
- **UserCard**: 4 props + 3 state + 2 handlers = 12 points → Advanced
- **SearchInput**: 3 props + 2 state + 1 API call + 2 handlers = 10 points → Advanced
- **Modal**: 5 props + 4 state + 3 handlers = 16 points → Advanced
- **DataTable**: 6 props + 4 state + 2 API calls + 3 handlers = 19 points → Advanced

### Escalate to Senior (21+ points)
- **Dashboard**: 8 props + 6 state + 4 API calls + 5 handlers = 31 points → Escalate
- **CompleteForm**: 10 props + 8 state + 3 API calls + 6 handlers = 35 points → Escalate

## Loading Protocol

### Step 1: Load This Guide (Always)
```markdown
Context: templates/template-selection-guide.md
Tokens: 15
Purpose: Determine template complexity
```

### Step 2: Assess Complexity (30 seconds max)
1. Count complexity factors
2. Calculate total score
3. Determine template category

### Step 3: Load Selected Template (Only One)
```markdown
# For Score 0-8
Load: templates/component-basic.svelte
Total Context: 40 tokens (15 + 25)

# For Score 9-20  
Load: templates/component-advanced.svelte
Total Context: 60 tokens (15 + 45)

# For Score 21+
Action: Escalate to senior agent
Message: "Component complexity exceeds junior scope ([score] points)"
```

## Critical Rules

### ✅ ALWAYS
- Load this guide first
- Calculate complexity score
- Load ONLY selected template
- Document template choice in task acknowledgment

### ❌ NEVER
- Load multiple templates
- Skip complexity assessment
- Use comprehensive template unless escalating
- Exceed context budget for template loading

## Template Switching Mid-Task

### When to Switch
- **Basic → Advanced**: Component exceeds 40 lines or complexity grows
- **Advanced → Escalate**: Requirements exceed junior agent scope

### Switching Process
1. Document switch reason: "Complexity increased due to [specific reason]"
2. Update context: Unload current template, load new template
3. Refactor existing code to match new template patterns
4. Update token budget accounting

## Uncertainty Handling

### If Uncertain About Complexity
1. **Start with Basic template**
2. **Monitor implementation progress**  
3. **Switch to Advanced if needed**
4. **Document decision rationale**

### If Task Scope Changes
1. **Reassess complexity immediately**
2. **Switch templates if score changes category**
3. **Notify senior agent if escalation needed**
4. **Update task acknowledgment with new approach**

This guide enables efficient template selection without loading unnecessary context.