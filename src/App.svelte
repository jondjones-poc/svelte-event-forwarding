<script>
	import { tick } from 'svelte';
	import Product from './Product.svelte'
	import Modal from './Modal.svelte'

	let displayModal= false;
	let closable = false;
	let text = "some text";

	let products = [
		{
			id: "p1",
			title: 'title 1',
			price: 9.99
		},{
			id: "p2",
			title: 'title 2 ',
			price: 4.99,
			bestSeller: true
		}
	];

	function addToCart(event) {
		console.log(event);
		alert(`Id ${event.detail.id}`);
	}

	function toogleModal() {
		displayModal = !displayModal;
	}

	function transform(event) {
	  if (event.which !== 9) {
		  return;
	  }
	  event.preventDefault();

	  const selectionStart = event.target.selectionStart;
	  const selectionEnd = event.target.selectionEnd;
	  const value = event.target.value

	  text = value.slice(0, selectionStart) +
			 value.slice(selectionStart, selectionEnd).toUpperCase() +
			 value.slice(selectionEnd);

	  tick().then(() => {
		event.target.selectionStart = selectionStart;
	    event.target.selectionEnd = selectionEnd;
	  });
	}
</script>

{#each products as product}	
	<Product 
	{...product} 
	on:click={() => alert('Click')} 
	on:add-to-cart={addToCart} 
	on:delete-from-cart={() => alert('Delete')} />
{/each}

<hr />
<button on:click={toogleModal}>
	Show Modal
</button>

{#if displayModal}
	<!-- Using props cloable defined here -->
	<!-- didGree defined in child component -->
	<Modal 
		on:close={() => displayModal = false }
		let:didAgree={closable}		
	>
		<h1 slot="header">Hi!</h1>
		<p>This Works!</p>
		<button 
			slot="footer" 
			on:click={() => (displayModal = false)}
			disabled={!closable}>
			Close
		</button>
	</Modal>
{/if}
<hr />
<textarea rows="5" value={text} on:keydown={transform} />
<div>
 Add text, highlight text and press tab to make it capital
</div>