<script setup>
    import { ref, watch } from "vue";
    import Keyboard from "@/Keyboard.vue"
    import InputBox from "@/InputBox.vue"
    import { arabic101Map, arabicWords, arabicSentences, getManyRandom, shuffleArray } from "@/helper.js";

    const gameStarted = ref(false);
    const gameMode = ref("words");
    const letterList = Object.values(arabic101Map);
    const activeKey = ref(null);
    const arText = ref("");
    const gameText = ref(null);
    const arrIndex = ref(0);
    const shuffledArr = ref(null);
    const numLeft = ref(0);
    const numTerms = ref(10);

    function startGame() {
	if (gameMode.value === "single") {
	    shuffledArr.value = shuffleArray(letterList);
	}

	else if (gameMode.value === "words") {
	    shuffledArr.value = getManyRandom(arabicWords, numTerms.value);
	}

	else if (gameMode.value === "sentences") {
	    shuffledArr.value = getManyRandom(arabicSentences, numTerms.value);
	}

	arrIndex.value = 0;
	arText.value = "";
	gameText.value = shuffledArr.value[arrIndex.value];
	numLeft.value = shuffledArr.value.length;
	gameStarted.value = true;
    }


    watch(() => arText.value, () => {
	if (gameStarted) {
	    if(arText.value === gameText.value) {
		arrIndex.value++;
		if (arrIndex.value >= shuffledArr.value.length) gameStarted.value = false;
		arText.value = "";
	    }
	}


	if(arText.value.length > gameText.value?.length - 1) arText.value = arText.value.slice(0, arText.value.length - gameText.value.length - 1);
    })

    watch(() => arrIndex.value, () => {
	gameText.value = shuffledArr.value[arrIndex.value];
	numLeft.value = (shuffledArr.value.length - arrIndex.value);
    })

    watch(numTerms, (val) => {
	if (val > 50) numTerms.value = 50;
	if (val < 1) numTerms.value = 1;
    });

</script>

<template>
    <div class="container">
	<div class="start-button-box" v-if="!gameStarted">
	    <div class="radio-buttons">
		<input
		    class="num-input"
		    type="number"
		    v-model.number="numTerms"
		    :disabled="gameMode === 'single'"
		>
		<div class="radio-button">
		    <input type="radio" id="single-id" value="single" v-model="gameMode" />
		    <label for="single-id">Single</label>
		</div>
		<div class="radio-button">
		    <input type="radio" id="words-id" value="words" v-model="gameMode" />
		    <label for="words-id">Words</label>
		</div>
		<div class="radio-button">
		    <input type="radio" id="sentences-id" value="sentences" v-model="gameMode" />
		    <label for="sentences-id">Sentences</label>
		</div>
	    </div>
	    <button class="start-button" @click="startGame">Start</button>
	</div>
	<div v-else>
	    <span class="num-left">Words Left: {{ numLeft }}</span>
	    <div class="game-text">
		<h2>{{ gameText }}</h2>
	    </div>
	</div>
	<InputBox
	    v-model:ar-text="arText"
	    v-model:active-key="activeKey"
	/>
	<Keyboard :active-key="activeKey" />
    </div>
</template>

<style scoped>
    .container {
	margin: 1rem auto;      /* my-4 mx-auto */
	max-width: 85rem;       /* max-w-[85rem] */
	padding-top: 4rem;      /* pt-16 */
    }
    
    .start-button-box {
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	row-gap: 20px;
    }

    .start-button {
	font-size: 3rem;
	padding: 16px;
	margin-bottom: 32px;
    }
    
    .radio-buttons {
	font-size: 1.5rem;
	display: flex;
	column-gap: 20px;
    }

    .radio-button {
	display: flex;
	column-gap: 8px;
    }

    .game-text {
	font-family: "ZahirArabic", serif;
	display: flex;
	font-size: 2.5rem;
	justify-content: center;
	align-items: center;
    }

    .num-input {
    width: 50px;
    text-align: center;
    }

    .num-left {
	display: flex;
	justify-content: center;
	font-size: 2rem;
    }

</style>
