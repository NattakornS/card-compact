<script lang="ts">
	import type { CardInfo } from './model/card';
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();
	export let cardInfo: CardInfo = {
		logo: '',
		memberNo: 0,
		memberName: '',
		description: '',
		name: ''
	};
	// $: if (!showModal) {
	// 	cardInfo = {
	// 		logo: '',
	// 		memberNo: 0,
	// 		memberName: '',
	// 		description: '',
	// 		name: ''
	// 	};
	// }
	export let showModal = false;

	async function handleFileUpload(e: any) {
		const file = e.target.files[0];
		const reader = new FileReader();
		reader.onloadend = function () {
			cardInfo.logo = reader.result;
			saveChanges();
		};
		reader.readAsDataURL(file);
	}

	function saveChanges() {
		showModal = false;
		dispatch('saveChanges', cardInfo);
		dispatch('showModal', false);
	}
	function toggleModal() {
		showModal = !showModal;
	}
</script>

<div class={`modal ${showModal ? 'is-active' : ''}`}>
	<div class="modal-background"></div>
	<div class="modal-card">
		<header class="modal-card-head">
			<p class="modal-card-title">Card Information</p>
			<button class="delete" aria-label="close" on:click={toggleModal}></button>
		</header>
		<section class="modal-card-body">
			<!-- Card Information Form -->
			<div class="field">
				<label class="label" for="cardNumber">Card Number</label>
				<div class="control">
					<input
						class="input"
						type="text"
						placeholder="Card Number"
						bind:value={cardInfo.memberNo}
					/>
				</div>
			</div>
			<!-- Add more fields as needed -->
			<div class="field">
				<label class="label" for="cardName">Card Name</label>
				<div class="control">
					<input class="input" type="text" placeholder="Card Name" bind:value={cardInfo.name} />
				</div>
			</div>
			<div class="field">
				<label class="label" for="description">description</label>
				<div class="control">
					<input
						class="input"
						type="text"
						placeholder="Card Name"
						bind:value={cardInfo.description}
					/>
				</div>
			</div>
			<div class="field">
				<label class="label" for="memberName">Member Name</label>
				<div class="control">
					<input
						class="input"
						type="text"
						placeholder="Card Name"
						bind:value={cardInfo.memberName}
					/>
				</div>
			</div>
			<div class="field">
				<label class="label" for="logo">Logo</label>
				<div class="control">
					<input
						class="input"
						type="file"
						accept="image/*"
						bind:value={cardInfo.logo}
						on:change={(e) => handleFileUpload(e)}
					/>
				</div>
			</div>
		</section>
		<footer class="modal-card-foot">
			<button class="button is-success" on:click={saveChanges}>Save changes</button>
			<button class="button" on:click={toggleModal}>Cancel</button>
		</footer>
	</div>
</div>
