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
    dispatch('removeItem', {id: id});
  }
</script>

<div on:click={showHideItemDetails}>
  <!-- <div id="removeButton" on:click={removeItem}>X</div> -->
  <p>Location: {location}</p>
  {#if isExpanded}
    <p>Total: ${total}</p>
    <p>Category: {category}</p>
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
    /* padding-right: 10px; */
    user-select: none;
    width: 20%;
    z-index: 0;
  }

  #removeButton {
    border-style: solid;
    border-color: gray;
    border-width: 1px;
    color: black;
    float: right;
    font-weight: bold;
    margin: 0%;
    padding: 5px;
    position: relative;
    text-align: center;
    user-select: none;
    width:10px;
  }
</style>