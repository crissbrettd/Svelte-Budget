<script>
    import Purchase from './Purchase.svelte';
    import Form from './Form.svelte';
    import Popout from './Popout.svelte';
    import ListView from './ListView.svelte';
    
    $: itemList = loadPurchaseList()
    let shouldShowForm = false;
    let shouldShowList = false;

    let component;
    let props;

    function handleItemAdd(event) {
      itemList = [...itemList, { 
      id: makeId(),
      location: event.detail.location,
      total: event.detail.total,
      category: event.detail.category
      }]
      savePurchaseList()
      shouldShowForm = false;
    }

    function makeId() {
      let ID = "";
      let characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
      for ( var i = 0; i < 12; i++ ) {
        ID += characters.charAt(Math.floor(Math.random() * 36));
      }
      return ID;
    }

    function handleCloseForm()  {
      shouldShowForm = false;
    }

    function handleCloseList()  {
      shouldShowList = false;
    }

    function handleRemovePurchase(event) {
      itemList = itemList
        .filter((item) => item.id != event.detail);
      savePurchaseList();
    }

    function loadPurchaseList() {
      const json = localStorage.getItem("purchase-list")
      try {
        if (json !== null) {
          return JSON.parse(json)
        }
        else {
          return [] 
        }
      }
      catch {
        console.log("Something went wrong reading from storage or past purchase list is empty.")
        console.log(localStorage)
        return []
      }
    }

    const listView = () => {
      component = ListView;
    }

    const form = () => {
      component = Form;
    }

    function savePurchaseList() {
      const json = JSON.stringify(itemList, null, 4);
      localStorage.setItem("purchase-list", json);
    }

    function clearPurchaseList() {
      localStorage.setItem("purchase-list", []);
      itemList = []
    }

    function displayPurchaseForm() {
      form()
      shouldShowForm = true
    }

    function displayPurchaseList() {
      listView()
      shouldShowList = true;
    }


</script>

<main>
    <h1>Budget</h1>
    <div id="mainPurchaseDiv">        
        <div id="buttonContainer">
            <button class="mainButton" on:click={displayPurchaseForm}>
                Add Purchase
            </button>
            <br/>
            <button class="mainButton" on:click={clearPurchaseList}>
                Clear Purchase List
            </button>
            <br/>
            <button class="mainButton" on:click={displayPurchaseList}>
                View Purchase List
            </button>
        </div>
    </div>
    
    {#if shouldShowForm}
      <Popout>
        <Form on:addItem={handleItemAdd} on:closeForm={handleCloseForm}/>
      </Popout>
    {/if}

    {#if shouldShowList}
      <Popout>
        <ListView on:removePurchase={handleRemovePurchase} on:closeList={handleCloseList}/>
      </Popout>
    {/if}
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 500px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

    .mainButton {
        color: #ff3e00;
        margin-top: 10px;
    }

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>