<style>
	button:focus { box-shadow: 0 0 1px 1px blue; }
</style>
<Modal bind:this="{modal}" title="Hello">
	Modal contents
	<button type="button" on:click="{() => modal.close()}">Close</button>
	<textarea></textarea>
	<button>button</button>
</Modal>

<button on:click="{() => modal.open()}">Show modal</button>

<script>
import Modal from './Modal';
let modal;



</script>
