<script>
    import Purchase from './Purchase.svelte';
    import Form from './Form.svelte';
    import Popout from './Popout.svelte';
    
    $: itemList = loadPurchaseList()
    let shouldShowForm = false;

    // let thing = loadPurchaseList()
    // console.log(thing)

    function showForm() {
        shouldShowForm = !shouldShowForm;
    }

    function handleItemAdd(event) {
        itemList = [...itemList, { 
        id: itemList.length,
        location: event.detail.location,
        total: event.detail.total,
        category: event.detail.category
        }]
        shouldShowForm = event.detail.showForm;
    }

    function handleCloseForm()  {
        showForm()
    }

    function handleRemovePurchase(event) {
        console.log(itemList);
        itemList = itemList
            .filter((element) => {return element.detail.id !== event.detail.id})
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

    function savePurchaseList() {
        const json = JSON.stringify(itemList, null, 4);
        localStorage.setItem("purchase-list", json);
        console.log("saved")
    }

    function clearPurchaseList() {
        localStorage.setItem("purchase-list", []);
        itemList = []
    }


</script>

<main>
    <h1>Budget</h1>
    <div id="mainPurchaseDiv">
        {#each itemList as item}
            <Purchase {...item} on:removeItem={handleRemovePurchase}/>
        {/each}
        
        <div id="buttonContainer">
            <button class="mainButton" on:click={showForm}>
                Add Purchase
            </button>
            <br />
            <button class="mainButton" on:click={savePurchaseList}>
                Save Purchase List
            </button>
            <br />
            <button class="mainButton" on:click={clearPurchaseList}>
                Clear Purchase List
            </button>
        </div>
    </div>
    
    {#if shouldShowForm}
        <Popout>
            <Form on:addItem={handleItemAdd} on:closeForm={handleCloseForm}/>
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