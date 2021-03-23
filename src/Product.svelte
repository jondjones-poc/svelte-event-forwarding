<script>
    import { createEventDispatcher } from 'svelte';

    export let title;
    export let price;
    export let bestSeller = false;   // optional props
    export let id;

    const dispatch = createEventDispatcher();

    function addToCart() {
        dispatch('add-to-cart', { id })
    }
</script>

<article>
    <h1>{title}</h1>
    <h1>£{price}</h1>

    {#if bestSeller}
        <div>
            BEST SELLER
        </div>
    {/if}

    <!-- on:click - forwards event. In this example it will get handled in App.svelte -->
    <button on:click>Action</button>
    <button on:click={addToCart}>Add To Cart</button>
    <button on:click={() => {dispatch('delete-from-cart', {id})}}>Remove</button>
</article>