# TDD Pure Functions Pattern for Vibe Coding

> Documentation-driven development workflow for creating reliable, testable functions

## Core Workflow

### Step 1: Documentation First
Write clear JSDoc documentation describing the function's purpose:

```javascript
/**
 * Extracts first and last name from a full name string
 * @param {string} fullName - Complete name string (e.g., "John Michael Smith")
 * @returns {Object} { firstName: string, lastName: string }
 * @throws {Error} When fullName is empty or invalid
 * 
 * @example
 * parseName("John Smith") // { firstName: "John", lastName: "Smith" }
 * parseName("Mary Jane Watson") // { firstName: "Mary", lastName: "Watson" }
 */
```

### Step 2: Generate Tests
Create comprehensive tests based on documentation:

```javascript
// src/utils/parseName.test.js
import { describe, it, expect } from 'vitest'
import { parseName } from './parseName.js'

describe('parseName', () => {
  it('should parse simple two-part names', () => {
    expect(parseName('John Smith')).toEqual({
      firstName: 'John',
      lastName: 'Smith'
    })
  })

  it('should handle multi-part names by taking first and last', () => {
    expect(parseName('Mary Jane Watson')).toEqual({
      firstName: 'Mary',
      lastName: 'Watson'
    })
  })

  it('should handle single names', () => {
    expect(parseName('Madonna')).toEqual({
      firstName: 'Madonna',
      lastName: ''
    })
  })

  it('should throw error for empty input', () => {
    expect(() => parseName('')).toThrow('Invalid name')
    expect(() => parseName('   ')).toThrow('Invalid name')
  })

  it('should trim whitespace', () => {
    expect(parseName('  John   Smith  ')).toEqual({
      firstName: 'John',
      lastName: 'Smith'
    })
  })
})
```

### Step 3: Implement Function
Build function to satisfy all tests:

```javascript
// src/utils/parseName.js
/**
 * Extracts first and last name from a full name string
 * @param {string} fullName - Complete name string
 * @returns {Object} { firstName: string, lastName: string }
 * @throws {Error} When fullName is empty or invalid
 */
export function parseName(fullName) {
  if (!fullName || typeof fullName !== 'string' || !fullName.trim()) {
    throw new Error('Invalid name: fullName must be a non-empty string')
  }

  const parts = fullName.trim().split(/\s+/)
  
  if (parts.length === 1) {
    return {
      firstName: parts[0],
      lastName: ''
    }
  }
  
  return {
    firstName: parts[0],
    lastName: parts[parts.length - 1]
  }
}
```

## AI Agent Workflow

### Prompt Templates

#### Step 1: Documentation Generation
```markdown
Write JSDoc documentation for a function that [describe purpose]:

Requirements:
- Clear purpose description
- Parameter types and descriptions  
- Return type and structure
- Error conditions
- Usage examples
- Pure function (no side effects)
```

#### Step 2: Test Generation
```markdown
Based on this JSDoc documentation, write comprehensive tests:

[paste documentation]

Requirements:
- Test all documented behavior
- Include edge cases and error conditions
- Use Vitest testing framework
- Aim for 100% coverage
- Test examples from documentation
```

#### Step 3: Implementation
```markdown
Implement this function to pass all tests:

Documentation:
[paste JSDoc]

Tests:
[paste test file]

Requirements:
- Pure function (no side effects)
- Passes all tests
- Follows documentation exactly
- Clean, readable implementation
```

## Integration with Svelte Components

### Component Helper Functions
```svelte
<!-- src/components/UserCard.svelte -->
<script>
  import { formatUserName, validateEmail } from '$lib/utils/userHelpers.js'
  
  export let user
  
  // Use pure functions for data transformation
  $: displayName = formatUserName(user, { includeTitle: true })
  $: emailValidation = validateEmail(user.email)
  $: canEdit = user.permissions.includes('edit') && emailValidation.isValid
</script>

<div class="user-card">
  <h3>{displayName}</h3>
  {#if !emailValidation.isValid}
    <span class="error">{emailValidation.error}</span>
  {/if}
</div>
```

### Utility Functions Library
```javascript
// src/lib/utils/userHelpers.js

/**
 * Formats user display name with optional title
 * @param {Object} user - User object with firstName, lastName, title
 * @param {Object} options - Formatting options
 * @param {boolean} options.includeTitle - Include professional title
 * @param {boolean} options.lastNameFirst - Format as "Last, First"
 * @returns {string} Formatted display name
 */
export function formatUserName(user, options = {}) {
  // Implementation follows tests
}

/**
 * Validates email format and domain
 * @param {string} email - Email address to validate
 * @returns {Object} { isValid: boolean, error: string | null }
 */
export function validateEmail(email) {
  // Implementation follows tests
}
```

## Storybook Integration

### Component Stories with Function Documentation
```javascript
// src/components/UserCard.stories.js
import UserCard from './UserCard.svelte'
import { formatUserName } from '$lib/utils/userHelpers.js'

export default {
  title: 'Components/UserCard',
  component: UserCard,
  parameters: {
    docs: {
      description: {
        component: 'User card component with helper functions for name formatting'
      }
    }
  }
}

// Document the helper functions used
export const WithFormattedName = {
  args: {
    user: {
      firstName: 'John',
      lastName: 'Smith',
      title: 'Dr.',
      email: 'john@example.com'
    }
  },
  parameters: {
    docs: {
      description: {
        story: `Uses formatUserName() helper: ${formatUserName.toString()}`
      }
    }
  }
}
```

## Quality Benefits

### Guaranteed Test Coverage
Every function automatically has:
- ✅ 100% code coverage
- ✅ Edge case testing
- ✅ Error condition handling
- ✅ Documentation examples verified

### Predictable AI Behavior
Pure functions are ideal for AI because:
- No hidden dependencies
- Predictable input/output
- Easy to test and verify
- Clear success criteria

### Maintainable Code
Documentation-driven approach ensures:
- Clear function purpose
- Explicit contracts
- Usage examples
- Error handling

## Agent Instructions Update

### For Junior Backend Agent
```markdown
## Pure Function Development
For all utility functions, follow TDD workflow:

1. Write JSDoc documentation first
2. Generate comprehensive tests
3. Implement function to pass tests
4. Verify 100% coverage

Always prefer pure functions over stateful operations.
Move side effects (DB, API, file system) to service layer.
```

### For Junior Frontend Agent  
```markdown
## Component Helper Functions
Extract complex logic into pure functions:

1. Document function purpose with JSDoc
2. Create tests in adjacent .test.js file
3. Implement pure function
4. Use function in component reactive statements

Keep components focused on rendering and user interaction.
Move data transformation to pure helper functions.
```

## Example Agent Interaction

```markdown
**Human**: Create a function to calculate user's age from birthdate

**Agent Step 1**: 
```javascript
/**
 * Calculates user's current age from birthdate
 * @param {string|Date} birthdate - User's birth date
 * @param {Date} [currentDate=new Date()] - Reference date for calculation
 * @returns {number} Age in years
 * @throws {Error} When birthdate is invalid or in the future
 */
```

**Agent Step 2**: [Generates comprehensive tests]

**Agent Step 3**: [Implements function to pass all tests]

**Result**: Fully documented, tested, working function with 100% coverage
```

This workflow transforms function development from "hope it works" to "guaranteed to work" while being perfect for AI-assisted development.
