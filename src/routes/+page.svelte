<script lang="ts">
	import 'bulma/css/bulma.css';
	import type { CardInfo } from './model/card';
	import QRCode from './components/QrCode.svelte';
	import Barcode from './components/BarCode.svelte';
	import CardInfoComp from './CardInfoComp.svelte';
	import { onMount } from 'svelte';
	let cardList: Array<CardInfo> = [];
	//     [
	// 	{
	// 		name: 'Shell',
	// 		description: 'Shell Member',
	// 		memberNo: 1234567890,
	// 		memberName: 'Nattakorn Sanpabopit',
	// 		logo: 'https://th.bing.com/th/id/R.3a127dee61ad6739bce769c93a181ae6?rik=poS4WUoc%2bJroFA&riu=http%3a%2f%2f1000logos.net%2fwp-content%2fuploads%2f2017%2f06%2fShell-Logo.png&ehk=MElwpOQsCOF6QwKJHeKeB2UW6N0Vt6d8pOjM9O6fff4%3d&risl=&pid=ImgRaw&r=0'
	// 	},
	// ];

	let modelVis = false;
	function showModal() {
		modelVis = true;
	}
	onMount(() => {
		const storedCardInfo = localStorage.getItem('cardInfo');
		if (storedCardInfo) {
			cardList = JSON.parse(storedCardInfo);
		}
		window.addEventListener('storage', () => {
			const updatedCardInfo = localStorage.getItem('cardInfo');
			if (updatedCardInfo) {
				cardList = JSON.parse(updatedCardInfo);
			}
		});
	});
	function deleteCard(index: number) {
		cardList.splice(index, 1);
		localStorage.setItem('cardInfo', JSON.stringify(cardList));
	}
	function saveChanges(event: any) {
		cardList = event.detail;
	}
</script>

<div class="print-container container is-fullheight p-2">
	<div class="columns is-multiline">
		<!-- for loop cardList -->
		{#each cardList as item, index (item.memberNo)}
			<div class="column is-4-desktop is-4-widescreen is-6-tablet is-12-mobile">
				<div class="card">
					<div class="card-image">
						<!-- <figure class="image">
							<img
								src="https://th.bing.com/th/id/R.89f6bc1850ad7db5dfbd9561d0707b48?rik=bw2skn0hX86eRQ&pid=ImgRaw&r=0"
								alt="Placeholder image"
							/>
						</figure>
						<figure class="image">
							<img
								src="https://th.bing.com/th/id/OIP.-IyvRKGStlP4eYgYi2Da0wHaDu?rs=1&pid=ImgDetMain"
								alt="Placeholder image"
							/>
						</figure> -->
						<figure class="image">
							<QRCode data={item.memberNo + ''} />
						</figure>
						<figure class="image">
							<Barcode data={item.memberNo + ''} />
						</figure>
					</div>
					<div class="card-content">
						<div class="media">
							<div class="media-left">
								<figure class="image is-48x48">
									<img src={item.logo} alt="Placeholder image" aria-hidden="true" />
								</figure>
							</div>
							<div class="media-content">
								<p class="title is-4">{item.memberNo}</p>
								<p class="subtitle is-6">{item.memberName}</p>
							</div>
						</div>
						<button
							class="button is-primary delete-button is-danger m-2"
							on:click={() => deleteCard(index)}
						>
							<span class="icon is-small">
								<i class="fas fa-trash"></i>
							</span>
						</button>
						<!-- <div class="content">
						Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus nec iaculis mauris. <a
							>@bulmaio</a
						>.
						<a href="#">#css</a> <a href="#">#responsive</a>
						<br />
						<time datetime="2016-1-1">11:09 PM - 1 Jan 2016</time>
					</div> -->
					</div>
				</div>
			</div>
		{/each}
	</div>
</div>
<div class="fab">
	<button class="button is-primary" on:click={showModal}>
		<span class="icon is-small">
			<i class="fas fa-plus"></i>
		</span>
		<span>Add</span>
	</button>
	<button class="button is-primary">
		<span class="icon is-small">
			<i class="fas fa-print"></i>
		</span>
	</button>
</div>
<CardInfoComp bind:showModal={modelVis} on:saveChanges={saveChanges}></CardInfoComp>

<style>
	.card {
		position: relative;
		/* Other styles */
	}

	.delete-button {
		position: absolute;
		bottom: 0;
		right: 0;
	}
	.fab {
		position: fixed;
		bottom: 20px;
		right: 20px;
		z-index: 100;
	}
	.card-image {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}
	.card-image figure {
		margin: 0;
		height: 80px;
		width: auto;
	}
	@media print {
		.print-container {
			width: 210mm;
			height: 297mm;
			padding: 20mm;
			margin: 10mm auto;
			border: 1px #d3d3d3 solid;
			border-radius: 5px;
			background: white;
			box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
		}
	}
</style>
