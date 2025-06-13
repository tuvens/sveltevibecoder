<!-- For complex, multi-concern components -->
<script>
  import { createEventDispatcher, onMount } from 'svelte'
  import ChildComponent from './ChildComponent.svelte'
  
  // Props (4+ or complex props)
  export let data = []
  export let config = {}
  export let onSave = null
  export let validation = null
  
  // Event dispatcher
  const dispatch = createEventDispatcher()
  
  // State (multiple variables)
  let loading = false
  let error = null
  let editMode = false
  let formData = {}
  
  // Complex computed values
  $: isValid = validation ? validation(formData) : true
  $: hasChanges = JSON.stringify(formData) !== JSON.stringify(data)
  $: canSave = isValid && hasChanges && !loading
  
  // Lifecycle and data fetching
  onMount(async () => {
    try {
      loading = true
      // Complex initialization
    } catch (e) {
      error = e.message
    } finally {
      loading = false
    }
  })
  
  // Multiple event handlers
  async function handleSave() {
    try {
      loading = true
      await onSave(formData)
      dispatch('saved', { data: formData })
      editMode = false
    } catch (e) {
      error = e.message
    } finally {
      loading = false
    }
  }
  
  function handleCancel() {
    formData = { ...data }
    editMode = false
    dispatch('cancelled')
  }
</script>

<!-- Complex template with multiple sections -->
<div class="advanced-component" class:edit-mode={editMode}>
  <!-- Error handling -->
  {#if error}
    <div class="error">{error}</div>
  {/if}
  
  <!-- Loading state -->
  {#if loading}
    <div class="loading">Processing...</div>
  {/if}
  
  <!-- Main content with complex conditionals -->
  {#if editMode}
    <form on:submit|preventDefault={handleSave}>
      <ChildComponent bind:data={formData} {config} />
      
      <div class="actions">
        <button type="submit" disabled={!canSave}>Save</button>
        <button type="button" on:click={handleCancel}>Cancel</button>
      </div>
    </form>
  {:else}
    <div class="display-mode">
      <ChildComponent {data} {config} readonly />
      <button on:click={() => editMode = true}>Edit</button>
    </div>
  {/if}
</div>

<style>
  .advanced-component {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    padding: 1rem;
  }
  
  .edit-mode {
    border: 2px solid blue;
  }
  
  .error {
    color: red;
    padding: 0.5rem;
    background: #ffe6e6;
  }
  
  .actions {
    display: flex;
    gap: 0.5rem;
    justify-content: flex-end;
  }
</style>
