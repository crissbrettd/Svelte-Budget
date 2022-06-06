<script>
  import { createEventDispatcher } from 'svelte';
  import Purchase from './Purchase.svelte';

  $: itemList = loadPurchaseList();

  const dispatch = createEventDispatcher();

  function loadPurchaseList() {
      const json = localStorage.getItem("purchase-list");
      try {
        if (json !== null) {
          return JSON.parse(json);
        }
        else {
          return [];
        }
      }
      catch {
        console.log("Something went wrong reading from storage or past purchase list is empty.");
        console.log(localStorage);
        return [];
    }
  }

  function removePurchase(event) {
    dispatch('removePurchase', event.detail);
    closeList();
  }

  function closeList() {
    dispatch('closeList')
  }

</script>

<form>
  <div id="exitButton" on:click={closeList}>X</div>
  <div id="purchaseList">
    {#if itemList.length != 0}
      {#each itemList as item}
        <Purchase {...item} on:removeItem={removePurchase}/>
      {/each}
    {/if}
    {#if itemList.length == 0}
      <p>No items</p>
    {/if}

  </div>
</form>

<style>

  #exitButton {
    border-style: solid;
    border-color: gray;
    border-top-right-radius: 1em;
    border-width: 1px;
    color: red;
    float: right;
    font-weight: bold;
    padding: 10px;
    user-select: none;
  }

  #purchaseList {
    padding-bottom: 45px;
    padding-top: 45px;
  }

</style>
