<script>
  import { createEventDispatcher } from 'svelte';
  let location = '';
  let total = ''
  let category = ''
  const categoryOptions = ["Food", "Entertainment", "Household", "Books"];

  const dispatch = createEventDispatcher();

  function addItem() {
    console.log(location, total, category)
      dispatch('addItem', {
        location: location,
        total: total,
        category: category,
        showForm: false
      })

    location = ''
    total = ''
    category = ''
  }

  function closeForm() {
    dispatch('closeForm')
  }
</script>

<form>
  <div id="exitButton" on:click={closeForm}>X</div>
  <div>
    <label for="location">Location:</label>
    <input
      type="text"
      id="location"
      name="location"
      bind:value={location}
    />
    <label for="totalCost">Total price:</label>
    <input
      type="number"
      id="totalCost"
      name="totalCost"
      bind:value={total} min = 0 max = 9999
    />
    <label for="itemName">Category:</label>
    <select multiple bind:value={category}>
      {#each categoryOptions as categoryItem}
        <option value={categoryItem}>
          {categoryItem}
        </option>
      {/each}
    </select>
  </div>
  <button on:click|preventDefault={addItem}>
    Submit
  </button>
</form>

<style>

  #exitButton {
    border-style: solid;
    border-color: gray;
    border-width: 1px;
    color: red;
    float: right;
    font-weight: bold;
    padding: 10px;
    user-select: none;
  }

  /* #exitButtton :active {
    background-color: gray;
  } */

  label {
    padding: 10px;
  }

</style>
