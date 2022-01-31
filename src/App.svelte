<script>
	import Inputmask from "inputmask";
	import creditCardType from "credit-card-type"

	let backface = false,
			numberInput,
			expireInput,
			cvvInput

	let card = {
		number: '',
		name: '',
		expire: '',
		cvv: ''
	}

	$: type = card.number ? creditCardType(card.number)?.[0]?.type : false

	$: if (numberInput && expireInput && cvvInput) {
		
		Inputmask({
			mask: '9999 9999 9999 9999',
			placeholder: ' '
		}).mask(numberInput)

		Inputmask({
			mask: '99/99',
			placeholder: ' '
		}).mask(expireInput)

		Inputmask({
			mask: '999',
			placeholder: ' '
		}).mask(cvvInput)
	}

	const addCard = () => {
		console.log(card);
	}

</script>


<div class="card" class:flip={backface}>
	<div class="front">
		<div class="card-top">
			<img src="img/chip.svg" alt="">
			{#if type}
			<img src="img/{type}.svg" alt="">
			{/if}
		</div>
		<div class="card-number">
			{card.number || '**** **** **** ****'}
		</div>
		<div class="card-bottom">
			<div>
				<div class="key">Card Holder Name</div>
				<div class="value">{card.name || '***'}</div>
			</div>
			<div>
				<div class="key">Expiry Date</div>
				<div class="value">{card.expire || '***'}</div>
			</div>
		</div>
	</div>
	<div class="back">
		<div class="card-back">
			CVV <em>{card.cvv || '***'}</em>
		</div>
	</div>
</div>

<input type="text" bind:this={numberInput} bind:value={card.number} placeholder="Card Number"><br>
<input type="text" bind:value={card.name} placeholder="Card Holder Name"><br>
<input type="text" bind:this={expireInput} bind:value={card.expire} placeholder="Expiry Date"><br>
<input type="text" bind:this={cvvInput} on:focus={() => backface = true} on:blur={() => backface = false} bind:value={card.cvv} placeholder="CVV"><br>
<label>
	<input type="checkbox" bind:checked={backface}>
	Back face {backface ? 'hide' : 'show'}
</label><br>

<button on:click={addCard}>Save</button>

<pre>{JSON.stringify(card)}</pre>

<style>
	.card {
		width: 352px;
		height: 223px;
		position: relative;
		perspective: 800px;
	}
	.card .front, .card .back{
		width: inherit;
		height: inherit;
		box-sizing: border-box;
		background: linear-gradient(31.58deg, #93278F -2.49%, #29ABE2 67.92%);
		border-radius: 15px;
		position: absolute;
		top: 0;
		left: 0;
		backface-visibility: hidden;
		transition: 1s all;
		padding: 31px 27px;
    	display: flex;
    	flex-direction: column;
	}
	.card .back{
		transform: rotateY(180deg);
	}
	.card .front .card-top {
		display: flex;
		justify-content: space-between;
	}
	.card .front .card-number {
		font-size: 27px;
		font-family: monospace;
		color: #fff;
		letter-spacing: -3px;
		margin-top: 25px;
	}

	.card .front .card-bottom {
		margin-top: auto;
		display: flex;
		justify-content: space-between;
		color: #fff;
	}
	.card .front .card-bottom .key {
		font-size: 13px;
		letter-spacing: -.3px;
		font-weight: 500;
		margin-bottom: 5px;
		opacity: .7;
	}
	.card .front .card-bottom .value {
		font-size: 18px;
		font-weight: 600;

	}

	.card .back .card-back{
		background-color: white;
		padding: 20px;
		margin-top: auto;
		display: flex;
		justify-content: end;
	}

	.card .back .card-back em {
		font-weight: bold;
		margin-left: 15px;
	}


	.card.flip .back{
		transform: rotateY(0);
	}
	.card.flip .front{
		transform: rotateY(-180deg);
	}

</style>