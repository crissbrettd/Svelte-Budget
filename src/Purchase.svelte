<script>
  import { createEventDispatcher } from 'svelte';
  export let id;
  export let location;
  export let total;
  export let category;

  let isExpanded = false;
  const dispatch = createEventDispatcher();

  function showHideItemDetails() {
    isExpanded = !isExpanded;
  }

  function removeItem() {
    dispatch('removeItem', id);
  }
</script>

<div on:click={showHideItemDetails}>
  <p>Location: {location}</p>
  {#if isExpanded}
    <p>Total: ${total}</p>
    <p>Category: {category}</p>
    <!-- <p>{id}</p> --> <!-- show id for testing-->
    <p id="removeButton" on:click={removeItem}>remove</p>
  {/if}
  
</div>

<style>
  div {
    background-color: #ff3e00;
    border-color: white;
    border-style: solid;
    border-width: 1px;
    color: white;
    margin: auto;
    margin-top: 10px;
    padding-left: 10px; 
    user-select: none;
    width: 80%;
    z-index: 0;
  }

  #removeButton {
    font-size: smaller;
    font-style: italic;
  }
</style>