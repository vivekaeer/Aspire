<template>	
	<div class="credit-card">	
		<div class="logosymbol"><img :src="'/img/Logo.svg'" /></div>
		<form @submit.prevent="addCard()">	
		<input
			v-model="newCard"
			name="newCard"
			autocomplete="off"
		>
		<button>Add Card</button>
	</form>	
	<div>
		<ul>
			<li class="card-front"  
				v-for="(card, index) in cards"
				:key="index"		>
				<div class="card-front" >		
					<div class="shift">	
						<img :src="'/img/AspireLogo.svg'">&nbsp;							
					</div>			
					<div class="card-front__number">					 
						<p :class="{ done: card.done, value}">{{card.cardName}}</p>
					</div>	
					<p :class="{ done: card.done, value}">{{card.cardNumber}}</p>			
					<div class="card-front__info left">
						<p>Thru</p>
						<p :class="{ done: card.done}">
						{{ card.expiryDate  }}
						</p>					 
					</div>	
					<div class="shift">	
						<img :src="'/img/VisaLogo.svg'">&nbsp;							
					</div>									 
					<div class="shift pointer">	
						<img :src="'/img/Deactivatecard.svg'" @click="removeCard(index)">&nbsp;
						<img :src="'/img/FreezeCard.svg'" @click="doneCard(card)">&nbsp;									 						
					</div>							
				</div>
			</li>
		</ul>
	</div>	
	<h4 v-if="cards.length === 0">Empty list.</h4>
	</div>	
</template>

<script>
	import { ref } from 'vue';
	export default {
		name: 'App',
		setup () {
			const newCard = ref('');
			const defaultData = [{
				done: false,
				cardName: 'Vivek Aair',
				cardNumber: '2345 3456 3456 3423',
				expiryDate: '03 / 28',
			}]
			const cardsData = JSON.parse(localStorage.getItem('cards')) || defaultData;
			const cards = ref(cardsData);
			function addCard () {
				if (newCard.value) {
					cards.value.push({
						done: false,
						cardName: newCard.value,
						cardNumber: cardNumberGeneration(),
						expiryDate: cardExpiryDate(),
					});
					newCard.value = '';
				}
				saveData();
			}

			function doneCard (card) {			
				card.done = !card.done
				saveData();
			}			

			function removeCard (index) {
				cards.value.splice(index, 1);
				saveData();
			}

			function cardNumberGeneration () {
				return ("" + Math.random()).substring(2, 6) + " " + ("" + Math.random()).substring(2, 6) + " " + ("" + Math.random()).substring(2, 6) + " " + ("" + Math.random()).substring(2, 6);
			}
			
			function cardExpiryDate () {
				let Year = new Date().getFullYear();
				let futureYear = Year + 5;
				let Month = new Date().getMonth();
				return (Month) + " / "  + (('' + futureYear).substr(2));
			}

			function saveData () {
				const storageData = JSON.stringify(cards.value);
				localStorage.setItem('cards', storageData);
			}

			return {
				cards,
				newCard,
				addCard,
				doneCard,
				removeCard,
				saveData
			}
		}
	}
</script>

<style lang="scss">
$border: 2px solid
	rgba(
		$color: white,
		$alpha: 0.35,
	);
$size1: 6px;
$size2: 12px;
$size3: 18px;
$size4: 24px;
$size5: 48px;
$backgroundColor: #0C365A;
$textColor: white;
$primaryColor: #a0a4d9;
$secondTextColor: #1f2023;
body {
	margin: 0;
	padding: 0;
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	background-color: $backgroundColor;
	color: $textColor;
	#app {
		max-width: 600px;
		margin-left: auto;
		margin-right: auto;
		padding: 20px;
		h1 {
			font-weight: bold;
			font-size: 28px;
			text-align: center;
		}
		form {
			display: flex;
			flex-direction: column;
			width: 100%;
			label {
				font-size: 14px;
				font-weight: bold;
			}
			input,
			button {
				height: $size5;
				box-shadow: none;
				outline: none;
				padding-left: $size2;
				padding-right: $size2;
				border-radius: $size1;
				font-size: 18px;
				margin-top: $size1;
				margin-bottom: $size2;
			}
			input {
				background-color: transparent;
				border: $border;
				color: inherit;
			}
		}
		button {
			cursor: pointer;
			background-color: $primaryColor;
			border: 1px solid $primaryColor;
			color: $secondTextColor;
			font-weight: bold;
			outline: none;
			border-radius: $size1;
		}
		h2 {
			font-size: 22px;
			border-bottom: $border;
			padding-bottom: $size1;
		}
		ul {
			padding: 10px;
			li {
				display: flex;
				justify-content: space-between;
				align-items: center;
				border: $border;
				padding: $size2 $size4;
				border-radius: $size1;
				margin-bottom: $size2;
				span {
					cursor: pointer;
				}
				.done {
					text-decoration: line-through;
				}
				button {
					font-size: $size2;
					padding: $size1;
				}
			}
		}
		h4 {
			text-align: center;
			opacity: 0.5;
			margin: 0;
		}
	}
}
 
$x-space: 24px;
$y-space: 16px;
 .logosymbol { 
    top: $y-space;
    right: $x-space;
    height: 48px;	
	text-align: right;  
  }
  .shift {
      float: right;
    }
  .pointer {
	cursor: pointer;
	}
.card-front {
  width: 100%;
  height: 100%;  
  backface-visibility: hidden;
  z-index: 100;
  background:#01D167;
  

  &__image {
    width: 100%;
    height: 100%;
    border-radius: 16px;
  }

  &__number {
 
    font-size: 26px;
    top: 35%;
    left: 24px;
  }

  &__chip {
 
    top: $y-space;
    left: $x-space;
    height: 44px;
  }

  &__symbol {
 
    top: $y-space;
    right: $x-space;
    height: 48px;
  }

  &__info {
 
    bottom: $y-space;
    right: $x-space;
    color: white;
    text-align: left;
    margin: 0;

    &.left {
      left: $x-space;
    }

    .value {
      font-weight: bold;
    }

    p {
      margin: 0;
    }
  }
  &__expires {
    right: auto;
    left: $x-space;
  }
  @media screen and (max-width: 480px) {
    &__number {
      font-size: 22px;
    }
    &__info {
      font-size: 12px;
    }
    &__chip {
      height: 34px;
    }
    &__symbol {
      height: 38px;
    }
  }
  @media screen and (max-width: 360px) {
    &__number {
      font-size: 18px;
    }
  }
}
.credit-card {
  font-family: "Open Sans", monospace;
  max-width: 420px;
  width: 100%;
  height: 245px;
  background-color: transparent;
  color: white;
  perspective: 1000px;
  display: inline-block;

  &:hover &__inner {
    transform: rotateY(180deg);
  }

  &__inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.6s;
    transform-style: preserve-3d;
  }

  @media screen and (max-width: 480px) {
    height: 210px;
  }

  @media screen and (max-width: 360px) {
    height: 180px;
  }
}
</style>
