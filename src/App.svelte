<script>
    import Purchase from './Purchase.svelte';
    import Form from './Form.svelte';
    import Popout from './Popout.svelte';
    $: itemList = []
    let shouldShowForm = false;

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
        itemList = itemList.filter((element) => {return element.detail.id !== event.detail.id})
    }

</script>

<main>
    <h1>Budget</h1>
    <div id="mainPurchaseDiv">
        {#each itemList as item}
            <Purchase {...item} on:removeItem={handleRemovePurchase}/>
        {/each}
        
        <button id="addPurchaseButton" on:click={showForm}>Add Purchase</button>
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

    #addPurchaseButton {
        color: #ff3e00;
        margin-top: 10px;
    }

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>