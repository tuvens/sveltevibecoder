# Component Architecture for Vibe Coding

> Scalable patterns for organizing Svelte 4 components in AI-assisted development

## Component Hierarchy Principles

### 1. Single Responsibility Components
Each component should have one clear purpose that can be described in a single sentence.

```svelte
<!-- ✅ GOOD: Clear, focused responsibility -->
<!-- src/components/UserAvatar.svelte -->
<script>
  export let user
  export let size = 'medium'
  
  $: imageSrc = user.avatar || '/default-avatar.png'
  $: sizeClass = `avatar-${size}`
</script>

<img 
  src={imageSrc} 
  alt={user.name}
  class={sizeClass}
/>

<!-- ❌ AVOID: Multiple responsibilities -->
<!-- UserProfile.svelte that handles avatar, form editing, AND notifications -->
```

### 2. Composition Over Inheritance
Build complex components by combining simpler ones.

```svelte
<!-- src/components/UserCard.svelte -->
<script>
  import UserAvatar from './UserAvatar.svelte'
  import Badge from './Badge.svelte'
  import Button from './Button.svelte'
  
  export let user
  export let showActions = true
</script>

<div class="user-card">
  <UserAvatar {user} size="large" />
  
  <div class="user-info">
    <h3>{user.name}</h3>
    {#if user.isVerified}
      <Badge variant="success">Verified</Badge>
    {/if}
  </div>
  
  {#if showActions}
    <div class="actions">
      <Button on:click={() => dispatch('edit', user)}>Edit</Button>
      <Button variant="secondary" on:click={() => dispatch('message', user)}>
        Message
      </Button>
    </div>
  {/if}
</div>
```

## Component Size Guidelines

### Small Components (< 50 lines)
Perfect for AI agents to implement in single tasks.

```svelte
<!-- src/components/LoadingSpinner.svelte -->
<script>
  export let size = 'medium'
  export let color = 'primary'
  
  $: classes = `spinner spinner-${size} spinner-${color}`
</script>

<div class={classes} />

<style>
  .spinner {
    border: 2px solid #f3f3f3;
    border-radius: 50%;
    border-top: 2px solid;
    animation: spin 1s linear infinite;
  }
  
  .spinner-small { width: 16px; height: 16px; }
  .spinner-medium { width: 24px; height: 24px; }
  .spinner-large { width: 32px; height: 32px; }
  
  .spinner-primary { border-top-color: blue; }
  .spinner-secondary { border-top-color: gray; }
  
  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
</style>
```

### Medium Components (50-150 lines)
Require planning but manageable for junior agents with clear specifications.

```svelte
<!-- src/components/SearchInput.svelte -->
<script>
  import { createEventDispatcher } from 'svelte'
  import LoadingSpinner from './LoadingSpinner.svelte'
  
  export let placeholder = 'Search...'
  export let loading = false
  export let debounceMs = 300
  
  const dispatch = createEventDispatcher()
  
  let query = ''
  let debounceTimer
  
  $: if (query !== undefined) {
    clearTimeout(debounceTimer)
    debounceTimer = setTimeout(() => {
      dispatch('search', { query })
    }, debounceMs)
  }
  
  function handleClear() {
    query = ''
    dispatch('clear')
  }
</script>

<div class="search-container">
  <input
    bind:value={query}
    {placeholder}
    disabled={loading}
    class="search-input"
  />
  
  <div class="search-actions">
    {#if loading}
      <LoadingSpinner size="small" />
    {:else if query}
      <button on:click={handleClear} class="clear-btn">×</button>
    {/if}
  </div>
</div>

<style>
  .search-container {
    position: relative;
    display: flex;
    align-items: center;
  }
  
  .search-input {
    width: 100%;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  .search-actions {
    position: absolute;
    right: 0.5rem;
  }
  
  .clear-btn {
    background: none;
    border: none;
    font-size: 1.2rem;
    cursor: pointer;
  }
</style>
```

### Large Components (150+ lines)
Should be broken down or require senior agent planning.

## State Management Patterns

### Local State (Component-Specific)
```svelte
<script>
  // Simple local state
  let isOpen = false
  let currentTab = 'general'
  let formData = { name: '', email: '' }
  
  // Computed local state
  $: isValid = formData.name && formData.email
  $: hasUnsavedChanges = JSON.stringify(formData) !== JSON.stringify(originalData)
</script>
```

### Lifted State (Parent-Child Communication)
```svelte
<!-- Parent: UserList.svelte -->
<script>
  let selectedUser = null
  let users = []
  
  function handleUserSelect(event) {
    selectedUser = event.detail.user
  }
</script>

{#each users as user}
  <UserCard 
    {user} 
    selected={selectedUser?.id === user.id}
    on:select={handleUserSelect} 
  />
{/each}

<!-- Child: UserCard.svelte -->
<script>
  import { createEventDispatcher } from 'svelte'
  
  export let user
  export let selected = false
  
  const dispatch = createEventDispatcher()
  
  function selectUser() {
    dispatch('select', { user })
  }
</script>

<div class="user-card" class:selected on:click={selectUser}>
  <!-- content -->
</div>
```

### Global State (Stores)
```svelte
<!-- src/lib/stores/user.js -->
import { writable } from 'svelte/store'

export const currentUser = writable(null)
export const userPreferences = writable({
  theme: 'light',
  language: 'en'
})

<!-- Component using global state -->
<script>
  import { currentUser } from '$lib/stores/user.js'
  
  $: if ($currentUser) {
    console.log('User logged in:', $currentUser.name)
  }
</script>
```

## Error Handling Patterns

### Component-Level Error Boundaries
```svelte
<!-- src/components/ErrorBoundary.svelte -->
<script>
  export let fallback = 'Something went wrong'
  
  let error = null
  let hasError = false
  
  // Reset error when content changes
  $: if (!hasError) {
    error = null
  }
  
  function handleError(event) {
    error = event.detail.error
    hasError = true
    console.error('Component error:', error)
  }
</script>

{#if hasError}
  <div class="error-fallback">
    <h3>Oops!</h3>
    <p>{fallback}</p>
    <button on:click={() => hasError = false}>Try Again</button>
  </div>
{:else}
  <div on:error={handleError}>
    <slot />
  </div>
{/if}
```

### Async Error Handling
```svelte
<script>
  import { onMount } from 'svelte'
  
  let data = null
  let loading = true
  let error = null
  
  async function fetchData() {
    try {
      loading = true
      error = null
      
      const response = await fetch('/api/data')
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`)
      }
      
      data = await response.json()
    } catch (e) {
      error = e.message
    } finally {
      loading = false
    }
  }
  
  onMount(fetchData)
</script>

{#if loading}
  <LoadingSpinner />
{:else if error}
  <div class="error">
    <p>Error: {error}</p>
    <button on:click={fetchData}>Retry</button>
  </div>
{:else if data}
  <DataDisplay {data} />
{:else}
  <p>No data available</p>
{/if}
```

## Performance Optimization Patterns

### Conditional Rendering for Performance
```svelte
<script>
  export let items = []
  export let showDetails = false
  
  // Only compute expensive operations when needed
  $: expensiveData = showDetails ? computeExpensiveData(items) : null
  
  function computeExpensiveData(items) {
    return items.map(item => ({
      ...item,
      analytics: calculateAnalytics(item)
    }))
  }
</script>

{#each items as item (item.id)}
  <div class="item">
    <h3>{item.name}</h3>
    
    {#if showDetails && expensiveData}
      <DetailView data={expensiveData.find(d => d.id === item.id)} />
    {/if}
  </div>
{/each}
```

### Event Delegation Pattern
```svelte
<script>
  export let items = []
  
  function handleItemAction(event) {
    const action = event.target.dataset.action
    const itemId = event.target.closest('[data-item-id]').dataset.itemId
    
    switch (action) {
      case 'edit':
        editItem(itemId)
        break
      case 'delete':
        deleteItem(itemId)
        break
    }
  }
</script>

<!-- Single event listener for all items -->
<div class="items-container" on:click={handleItemAction}>
  {#each items as item (item.id)}
    <div class="item" data-item-id={item.id}>
      <h3>{item.name}</h3>
      <button data-action="edit">Edit</button>
      <button data-action="delete">Delete</button>
    </div>
  {/each}
</div>
```

## Testing Architecture

### Component Testing Strategy
```svelte
<!-- src/components/Counter.svelte -->
<script>
  export let initialValue = 0
  export let step = 1
  export let min = null
  export let max = null
  
  let count = initialValue
  
  $: canIncrement = max === null || count < max
  $: canDecrement = min === null || count > min
  
  function increment() {
    if (canIncrement) count += step
  }
  
  function decrement() {
    if (canDecrement) count -= step
  }
</script>

<div class="counter">
  <button 
    on:click={decrement} 
    disabled={!canDecrement}
    data-testid="decrement"
  >
    -
  </button>
  
  <span data-testid="count">{count}</span>
  
  <button 
    on:click={increment} 
    disabled={!canIncrement}
    data-testid="increment"
  >
    +
  </button>
</div>
```

```javascript
// src/tests/Counter.test.js
import { render, fireEvent } from '@testing-library/svelte'
import { expect, test } from 'vitest'
import Counter from '../components/Counter.svelte'

test('increments and decrements count', async () => {
  const { getByTestId } = render(Counter, { 
    props: { initialValue: 5 } 
  })
  
  const count = getByTestId('count')
  const increment = getByTestId('increment')
  const decrement = getByTestId('decrement')
  
  expect(count).toHaveTextContent('5')
  
  await fireEvent.click(increment)
  expect(count).toHaveTextContent('6')
  
  await fireEvent.click(decrement)
  expect(count).toHaveTextContent('5')
})

test('respects min and max bounds', async () => {
  const { getByTestId } = render(Counter, {
    props: { initialValue: 0, min: 0, max: 2 }
  })
  
  const decrement = getByTestId('decrement')
  const increment = getByTestId('increment')
  
  expect(decrement).toBeDisabled()
  
  await fireEvent.click(increment)
  await fireEvent.click(increment)
  
  expect(increment).toBeDisabled()
})
```

## File Organization

### Recommended Structure
```
src/
├── components/
│   ├── ui/               # Reusable UI components
│   │   ├── Button.svelte
│   │   ├── Input.svelte
│   │   └── Modal.svelte
│   ├── layout/           # Layout components
│   │   ├── Header.svelte
│   │   ├── Sidebar.svelte
│   │   └── Footer.svelte
│   └── features/         # Feature-specific components
│       ├── user/
│       │   ├── UserCard.svelte
│       │   ├── UserList.svelte
│       │   └── UserForm.svelte
│       └── auth/
│           ├── LoginForm.svelte
│           └── SignupForm.svelte
├── lib/
│   ├── stores/           # Global state
│   ├── utils/            # Helper functions
│   └── types/            # TypeScript types
└── routes/               # SvelteKit pages
```

## AI Agent Guidelines

### For Junior Agents
- Focus on single-responsibility components (< 50 lines)
- Use provided templates and patterns consistently
- Include data-testid attributes for testing
- Follow established naming conventions
- Ask for clarification if component scope is unclear

### For Senior Agents
- Plan component hierarchy before implementation
- Define clear interfaces between components
- Consider performance implications of state management choices
- Establish error handling strategies
- Review and refactor large components into smaller ones

### Component Handoff Template
```markdown
## Component: [Name]

**Responsibility**: Single sentence describing what this component does
**Size Estimate**: Small/Medium/Large (< 50 / 50-150 / 150+ lines)
**Dependencies**: List of imported components
**Props Interface**: TypeScript interface or clear prop list
**Events**: Custom events this component dispatches
**State**: Local state variables needed
**Testing Requirements**: Key behaviors to test
```