# Svelte 4 Basics for Vibe Coding

> Essential patterns and syntax for AI-assisted Svelte 4 development

## Core Reactive Patterns

### Variables and State
```svelte
<!-- src/components/Counter.svelte -->
<script>
  // Simple reactive variable (4 tokens)
  let count = 0
  
  // Reactive assignment
  let items = []
  
  // Props from parent (export let)
  export let initialValue = 0
  export let disabled = false
</script>
```

### Computed Values (Reactive Statements)
```svelte
<script>
  let count = 0
  
  // Single computed value (6 tokens)
  $: doubled = count * 2
  
  // Conditional computed value
  $: isEven = count % 2 === 0
  
  // Complex computation
  $: summary = {
    count,
    doubled,
    status: count > 10 ? 'high' : 'low'
  }
  
  // Dependent computation
  $: squared = doubled * doubled
</script>
```

### Side Effects
```svelte
<script>
  let user = null
  
  // Simple side effect (8 tokens)
  $: if (user) console.log('User loaded:', user.name)
  
  // Block syntax for multiple statements
  $: {
    if (user && user.preferences) {
      document.title = `Welcome ${user.name}`
      localStorage.setItem('lastUser', user.id)
    }
  }
  
  // Cleanup pattern
  $: if (user) {
    const cleanup = setupUserSession(user)
    return cleanup
  }
</script>
```

## Component Structure (Token-Optimized)

### Standard Component Layout
```svelte
<!-- src/components/UserCard.svelte -->
<script>
  // 1. Imports (if needed)
  import { createEventDispatcher } from 'svelte'
  
  // 2. Props
  export let user
  export let editable = false
  
  // 3. Local state
  let editing = false
  let changes = {}
  
  // 4. Event dispatcher
  const dispatch = createEventDispatcher()
  
  // 5. Reactive statements
  $: hasChanges = Object.keys(changes).length > 0
  $: canSave = hasChanges && user
  
  // 6. Functions
  function startEdit() {
    editing = true
    changes = { ...user }
  }
  
  function saveChanges() {
    dispatch('save', changes)
    editing = false
  }
</script>

<!-- 7. Template -->
<div class="user-card">
  <h3>{user.name}</h3>
  
  {#if editing}
    <input bind:value={changes.name} />
    <button on:click={saveChanges} disabled={!canSave}>Save</button>
  {:else}
    <p>{user.email}</p>
    {#if editable}
      <button on:click={startEdit}>Edit</button>
    {/if}
  {/if}
</div>

<!-- 8. Styles -->
<style>
  .user-card {
    padding: 1rem;
    border: 1px solid #ccc;
  }
</style>
```

## Event Handling

### Event Syntax (Svelte 4 ONLY)
```svelte
<script>
  let count = 0
  
  function increment() {
    count++
  }
  
  function handleInput(event) {
    console.log(event.target.value)
  }
</script>

<!-- Correct Svelte 4 syntax -->
<button on:click={increment}>+</button>
<button on:click={() => count++}>+ Inline</button>
<input on:input={handleInput} />

<!-- WRONG: Svelte 5 syntax -->
<!-- <button onclick={increment}>+</button> -->
```

### Custom Events
```svelte
<!-- Child component -->
<script>
  import { createEventDispatcher } from 'svelte'
  
  const dispatch = createEventDispatcher()
  
  function notify() {
    dispatch('message', {
      text: 'Hello from child'
    })
  }
</script>

<button on:click={notify}>Send Message</button>

<!-- Parent component -->
<script>
  function handleMessage(event) {
    console.log(event.detail.text)
  }
</script>

<ChildComponent on:message={handleMessage} />
```

## Conditional Rendering

### If Blocks
```svelte
<script>
  let user = null
  let loading = false
</script>

<!-- Simple conditional -->
{#if user}
  <p>Welcome {user.name}!</p>
{/if}

<!-- If-else -->
{#if loading}
  <p>Loading...</p>
{:else if user}
  <UserProfile {user} />
{:else}
  <LoginForm />
{/if}

<!-- Inline conditional (for simple cases) -->
<p>{user ? `Welcome ${user.name}` : 'Please log in'}</p>
```

## List Rendering

### Each Blocks
```svelte
<script>
  let items = [
    { id: 1, name: 'Apple' },
    { id: 2, name: 'Banana' }
  ]
</script>

<!-- Basic each loop -->
{#each items as item}
  <div>{item.name}</div>
{/each}

<!-- With index -->
{#each items as item, i}
  <div>{i + 1}. {item.name}</div>
{/each}

<!-- With key (important for performance) -->
{#each items as item (item.id)}
  <ItemCard {item} />
{/each}

<!-- With else (empty state) -->
{#each items as item}
  <div>{item.name}</div>
{:else}
  <p>No items found</p>
{/each}
```

## Store Patterns

### Writable Stores
```svelte
<!-- src/lib/stores.js -->
import { writable } from 'svelte/store'

export const user = writable(null)
export const theme = writable('light')

<!-- Component using store -->
<script>
  import { user } from '$lib/stores.js'
  
  // Auto-subscription with $
  $: if ($user) {
    console.log('User changed:', $user.name)
  }
  
  function logout() {
    user.set(null)
  }
</script>

<p>Current user: {$user?.name || 'None'}</p>
<button on:click={logout}>Logout</button>
```

### Derived Stores
```svelte
<!-- src/lib/stores.js -->
import { writable, derived } from 'svelte/store'

export const user = writable(null)
export const isLoggedIn = derived(user, $user => !!$user)
export const userName = derived(user, $user => $user?.name || 'Guest')

<!-- Component -->
<script>
  import { isLoggedIn, userName } from '$lib/stores.js'
</script>

{#if $isLoggedIn}
  <p>Hello {$userName}!</p>
{:else}
  <p>Please log in</p>
{/if}
```

## Form Handling

### Two-Way Binding
```svelte
<script>
  let formData = {
    name: '',
    email: '',
    age: 0,
    newsletter: false
  }
  
  $: isValid = formData.name && formData.email
  
  function handleSubmit() {
    if (isValid) {
      console.log('Submitting:', formData)
    }
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  <input 
    bind:value={formData.name} 
    placeholder="Name" 
    required 
  />
  
  <input 
    type="email"
    bind:value={formData.email} 
    placeholder="Email" 
    required 
  />
  
  <input 
    type="number"
    bind:value={formData.age} 
    min="0" 
  />
  
  <label>
    <input 
      type="checkbox"
      bind:checked={formData.newsletter} 
    />
    Subscribe to newsletter
  </label>
  
  <button type="submit" disabled={!isValid}>
    Submit
  </button>
</form>
```

## Lifecycle Functions

### Component Lifecycle
```svelte
<script>
  import { onMount, onDestroy, beforeUpdate, afterUpdate } from 'svelte'
  
  let data = null
  
  // Runs after component first renders
  onMount(async () => {
    const response = await fetch('/api/data')
    data = await response.json()
    
    // Return cleanup function
    return () => {
      console.log('Component unmounted')
    }
  })
  
  // Explicit cleanup
  onDestroy(() => {
    console.log('Cleaning up...')
  })
  
  // Before DOM updates
  beforeUpdate(() => {
    console.log('About to update')
  })
  
  // After DOM updates
  afterUpdate(() => {
    console.log('Updated')
  })
</script>
```

## Common Patterns for AI Agents

### Token-Efficient Data Fetching
```svelte
<script>
  import { onMount } from 'svelte'
  
  let data = null
  let loading = true
  let error = null
  
  onMount(async () => {
    try {
      const response = await fetch('/api/users')
      data = await response.json()
    } catch (e) {
      error = e.message
    } finally {
      loading = false
    }
  })
</script>

{#if loading}
  Loading...
{:else if error}
  Error: {error}
{:else}
  <UserList users={data} />
{/if}
```

### Reusable Component Pattern
```svelte
<!-- src/components/Button.svelte -->
<script>
  export let variant = 'primary'
  export let disabled = false
  export let loading = false
  
  $: classes = `btn btn-${variant}`
</script>

<button 
  class={classes}
  {disabled}
  on:click
  {...$$restProps}
>
  {#if loading}
    Loading...
  {:else}
    <slot />
  {/if}
</button>

<style>
  .btn {
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .btn-primary {
    background: blue;
    color: white;
  }
  
  .btn:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
</style>
```

## Critical Reminders for AI Agents

### ✅ DO (Svelte 4)
- Use `let variable = value` for reactive variables
- Use `$: computed = expression` for computed values
- Use `on:event={handler}` for event binding
- Use `export let prop` for component props
- Use `{#if}{:else}{/if}` for conditionals
- Use `{#each items as item}{/each}` for loops

### ❌ AVOID (Svelte 5 syntax)
- NO `$state()`, `$derived()`, `$effect()`
- NO `onclick={}` event syntax
- NO runes of any kind

### Token Optimization Tips
- Prefer inline event handlers for simple actions
- Use reactive statements over functions when possible
- Keep component structure consistent
- Use destructuring in each loops when helpful
- Combine related reactive statements when logical
