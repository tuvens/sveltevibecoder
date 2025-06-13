# Svelte 4 Basics for Vibe Coding (Condensed)

> Essential Svelte 4 patterns optimized for AI agent consumption

## Quick Syntax Reference

| Pattern | Svelte 4 Syntax | Token Count | Use Case |
|---------|----------------|-------------|----------|
| Variable | `let count = 0` | 4 | Reactive state |
| Computed | `$: doubled = count * 2` | 6 | Derived values |
| Props | `export let user` | 3 | Component input |
| Events | `on:click={handler}` | 3 | User interactions |
| Binding | `bind:value={text}` | 3 | Two-way data |
| Conditional | `{#if condition}` | 3 | Show/hide content |
| Loop | `{#each items as item}` | 5 | Render lists |
| Store | `$store` | 2 | Global state |

## Component Structure Template

```svelte
<script>
  // 1. Imports
  import { createEventDispatcher } from 'svelte'
  
  // 2. Props
  export let data = []
  export let disabled = false
  
  // 3. Local state
  let loading = false
  
  // 4. Event dispatcher
  const dispatch = createEventDispatcher()
  
  // 5. Reactive statements
  $: isValid = data.length > 0 && !disabled
  
  // 6. Functions
  function handleClick() {
    dispatch('action', { data })
  }
</script>

<div class="component">
  {#if loading}
    Loading...
  {:else}
    <button on:click={handleClick} {disabled}>
      Action ({data.length})
    </button>
  {/if}
</div>

<style>
  .component { padding: 1rem; }
</style>
```

## Essential Patterns

### Reactive Patterns
```svelte
<script>
  let count = 0
  
  // Computed value
  $: doubled = count * 2
  
  // Conditional computed
  $: status = count > 10 ? 'high' : 'low'
  
  // Side effect
  $: if (count > 0) console.log(count)
  
  // Block syntax for multiple statements
  $: {
    if (count > 5) {
      document.title = `Count: ${count}`
    }
  }
</script>
```

### Event Handling
```svelte
<script>
  let value = ''
  
  function handleSubmit() {
    dispatch('submit', { value })
  }
</script>

<!-- Event binding -->
<button on:click={handleSubmit}>Submit</button>
<button on:click={() => count++}>Increment</button>
<input on:input={e => value = e.target.value} />

<!-- Event modifiers -->
<form on:submit|preventDefault={handleSubmit}>
<button on:click|once={handler}>Click Once</button>
```

### Conditional & Lists
```svelte
<script>
  let user = null
  let items = [{id: 1, name: 'Item'}]
</script>

<!-- Conditionals -->
{#if user}
  <p>Hello {user.name}</p>
{:else}
  <p>Please log in</p>
{/if}

<!-- Lists with key -->
{#each items as item (item.id)}
  <div>{item.name}</div>
{:else}
  <p>No items</p>
{/each}
```

### Form Binding
```svelte
<script>
  let formData = {
    name: '',
    email: '',
    age: 0,
    active: false
  }
  
  $: isValid = formData.name && formData.email
</script>

<input bind:value={formData.name} placeholder="Name" />
<input type="email" bind:value={formData.email} />
<input type="number" bind:value={formData.age} />
<input type="checkbox" bind:checked={formData.active} />

<button disabled={!isValid}>Submit</button>
```

### Store Usage
```svelte
<!-- stores.js -->
<script>
  import { writable, derived } from 'svelte/store'
  
  export const user = writable(null)
  export const isLoggedIn = derived(user, $user => !!$user)
</script>

<!-- Component -->
<script>
  import { user, isLoggedIn } from './stores.js'
</script>

{#if $isLoggedIn}
  <p>Welcome {$user.name}</p>
{/if}

<button on:click={() => user.set(null)}>Logout</button>
```

### Lifecycle & Data Fetching
```svelte
<script>
  import { onMount } from 'svelte'
  
  let data = null
  let loading = true
  let error = null
  
  onMount(async () => {
    try {
      const response = await fetch('/api/data')
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
  <DataDisplay {data} />
{/if}
```

## Component Communication

### Props & Events
```svelte
<!-- Parent -->
<script>
  let selectedUser = null
  
  function handleUserSelect(event) {
    selectedUser = event.detail.user
  }
</script>

<UserCard 
  user={selectedUser} 
  editable={true}
  on:select={handleUserSelect}
  on:save={handleSave}
/>

<!-- Child: UserCard -->
<script>
  import { createEventDispatcher } from 'svelte'
  
  export let user
  export let editable = false
  
  const dispatch = createEventDispatcher()
  
  function selectUser() {
    dispatch('select', { user })
  }
</script>
```

### Slots
```svelte
<!-- Component with slots -->
<div class="card">
  <header>
    <slot name="header">Default Header</slot>
  </header>
  <main>
    <slot />
  </main>
  <footer>
    <slot name="footer" {user}>
      <button>Default Action</button>
    </slot>
  </footer>
</div>

<!-- Usage -->
<Card>
  <h1 slot="header">Custom Header</h1>
  <p>Main content</p>
  <div slot="footer" let:user>
    <button>Action for {user.name}</button>
  </div>
</Card>
```

## Critical Rules for AI Agents

### ✅ ALWAYS Use (Svelte 4)
- `let variable = value` for reactive variables
- `$: computed = expression` for computed values
- `on:event={handler}` for event binding
- `export let prop` for component props
- `{#if}{:else}{/if}` for conditionals
- `{#each items as item (item.id)}{/each}` for loops

### ❌ NEVER Use (Svelte 5)
- `$state()`, `$derived()`, `$effect()` - These are Svelte 5 runes
- `onclick={}` - Use `on:click={}` instead
- Any rune syntax

### Token Optimization
- Prefer `$: doubled = count * 2` over function declarations
- Use inline event handlers for simple actions: `on:click={() => count++}`
- Combine related reactive statements when logical
- Use destructuring in loops: `{#each users as {id, name}}`

### Testing Essentials
- Always add `data-testid` attributes for testing
- Use consistent naming: `data-testid="component-name"`
- Include test IDs for interactive elements

```svelte
<div data-testid="user-card">
  <button data-testid="edit-button" on:click={edit}>Edit</button>
  <span data-testid="user-name">{user.name}</span>
</div>
```

### Performance Patterns
- Use keyed each blocks: `{#each items as item (item.id)}`
- Avoid creating functions in templates: `{#each items as item}<Button on:click={() => select(item)} />{/each}`
- Prefer: `<Button on:click={selectItem} data-id={item.id} />`

## Quick Troubleshooting

| Issue | Cause | Fix |
|-------|--------|-----|
| Reactivity not working | Assignment issue | Use `items = items` after push/splice |
| Events not firing | Wrong syntax | Use `on:event` not `onevent` |
| Props undefined | Export missing | Add `export let propName` |
| Store not reactive | Missing $ | Use `$store` not `store` |
| Computed not updating | Not using $ | Use `$: computed = expression` |

This condensed reference provides all essential Svelte 4 patterns needed for AI-assisted development in a token-efficient format.
