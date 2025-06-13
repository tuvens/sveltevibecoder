<!-- For simple, single-purpose components -->
<script>
  // Props (1-3 simple props)
  export let data
  export let disabled = false
  
  // State (1-2 variables max)
  let loading = false
  
  // Simple computed
  $: isValid = data && !disabled
  
  // Simple handler
  function handleAction() {
    dispatch('action', { data })
  }
</script>

<div class="component" class:disabled>
  {#if loading}
    Loading...
  {:else}
    <button on:click={handleAction} {disabled}>
      {data}
    </button>
  {/if}
</div>

<style>
  .component { padding: 0.5rem; }
  .disabled { opacity: 0.5; }
</style>

## Example 1: Basic Template Selection
### Task: Display User Avatar
**Assessment**: 
- Props: 2 (user, size) = 2 points
- State: 1 (imageLoaded) = 2 points  
- Events: 1 (onError) = 1 point
- **Total: 5 points → BASIC template**

**Context Loading**:
- Template: component-basic.svelte
- Patterns: svelte4-quick-reference.md
- **Token usage**: ~25 tokens
