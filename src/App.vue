<script setup>
    import { ref, watch } from "vue";
    import Keyboard from "@/Keyboard.vue"
    import InputBox from "@/InputBox.vue"
    import { arabic101Map, getRandomElement } from "@/helper.js";

    const gameStarted = ref(false);
    const letterList = Object.values(arabic101Map);
    const activeKey = ref(null);
    const arText = ref("");
    const gameText = ref(getRandomElement(letterList));


    watch(() => arText.value, () => {

	if(arText.value === gameText.value) {
	    gameText.value = getRandomElement(letterList);
	    arText.value = "";
	}

	if(arText.value.length > gameText.value.length - 1) arText.value = arText.value.slice(0, arText.value.length - gameText.value.length - 1);
    })

</script>

<template>
    <div class="container">
	<div class="start-button-box" v-if="!gameStarted">
	    <button class="start-button" @click="gameStarted = true">Start</button>
	</div>
	<div v-else class="game-text">
	    <h2>{{ gameText }}</h2>
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
	justify-content: center;
	align-items: center;
    }

    .start-button {
	font-size: 3rem;
	padding: 16px;
	margin-bottom: 32px;
    }

    .game-text {
	font-family: "ZahirArabic", serif;
	display: flex;
	font-size: 2.5rem;
	justify-content: center;
	align-items: center;
    }

</style>
