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
	export let showModal = false;

  async function handleFileUpload(e) {
    const file = e.target.files[0];
    const reader = new FileReader();
    reader.onloadend = function() {
      cardInfo.logo = reader.result;
      saveChanges();
    }
    reader.readAsDataURL(file);
  }

	function saveChanges() {
    let storedCardInfo = localStorage.getItem('cardInfo');
    if (!storedCardInfo) {
      storedCardInfo = "[]"
    }
      let cardInfos = JSON.parse(storedCardInfo);
      cardInfos.push(cardInfo)
      localStorage.setItem('cardInfo', JSON.stringify(cardInfos));
      showModal = false;
      dispatch('saveChanges', cardInfos);
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
				<label class="label">Card Number</label>
				<div class="control">
					<input class="input" type="text" placeholder="Card Number"   bind:value={cardInfo.memberNo} />
				</div>
			</div>
			<!-- Add more fields as needed -->
			<div class="field">
				<label class="label">Card Name</label>
				<div class="control">
					<input class="input" type="text" placeholder="Card Name"  bind:value={cardInfo.name} />
				</div>
			</div>
			<div class="field">
				<label class="label">DESC</label>
				<div class="control">
					<input class="input" type="text" placeholder="Card Name"  bind:value={cardInfo.description} />
				</div>
			</div>
			<div class="field">
				<label class="label">Member Name</label>
				<div class="control">
					<input class="input" type="text" placeholder="Card Name"  bind:value={cardInfo.memberName} />
				</div>
			</div>
			<div class="field">
				<label class="label">Logo</label>
				<div class="control">
					<input
						class="input"
						type="file"
						accept="image/*"
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

