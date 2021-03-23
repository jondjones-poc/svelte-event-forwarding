<script>
    import { createEventDispatcher, 
        onMount, onDestroy,
    beforeUpdate, afterUpdate } from 'svelte'
    const dispatch = createEventDispatcher();
    let agreed = false;
    let autoScroll = false;

    onMount(() => {
        console.log("onMount");
    });

    onDestroy(() => {
        console.log("onDestroy");
    });

    console.log('Script Executed')

    
    afterUpdate(() => {
        console.log('After Update');
        if (autoScroll) {
            const modal = document.querySelector('.modal');
            modal.scrollTo(0, modal.scrollHeight);
        }
    })


    beforeUpdate(() => {
        console.log('Before Update');
        autoScroll = agreed
    })
</script>

<style>
    .backdrop {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100vh;
        background: rgba(0, 0, 0, 0.75);
        z-index: 10;
    }

    .modal {
        padding: 1rem;
        position: fixed;
        top: 10vh;
        left: 10%;
        width: 80%;
        max-height: 30vh;
        background: white;
        border-radius: 5px;
        z-index: 100;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
        overflow: scroll;
    }

    header {
        border-bottom: 1px solid #ccc;
    }
</style>

<div class="backdrop"></div>

<!-- Uses HTML slot to pass content down -->
<div class="modal" on:click={() => dispatch('cancel')}>
    <header>
        <!-- Named slot -->
        <slot name="header" />
    </header>
    <div class="content">
        <slot />
    </div>
    <div class="disclaimer">
        <p>
            Before you close, you need to agree to our terms!
            <button on:click={() => agreed = true}>Agree</button>
        </p>
    </div>
    <footer>
        <!-- Default slot values with a prop-->
        <slot name="footer" didAgree={agreed}>
            <button on:click={() => dispatch('close')} disabled={!agreed}>Close</button>
        </slot>
    </footer>
</div>