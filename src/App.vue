<script setup>
    import { ref } from "vue";
    import Keyboard from "@/Keyboard.vue"
    import { arabic101Map } from "@/helper.js";

    const activeKey = ref(null);
    const arText = ref("");

    let clearTimer = null;

    function handleKey(event) {
	activeKey.value = event.key;

	// cancel previous timeout
	if (clearTimer) clearTimeout(clearTimer);

	clearTimer = setTimeout(() => {
	    activeKey.value = null;
	    clearTimer = null;
	}, 150);
    }

    function translateKey(event) {
	const key = event.key;

	// Allow control keys
	if (key.length > 1) return;

	const lower = key.toLowerCase();

	// If key exists in Arabic map → insert Arabic
	if (arabic101Map[lower]) {
	    arText.value += arabic101Map[lower];
	}
	// If user is typing Arabic keyboard, keep original
	else {
	    arText.value += key;
	}
    }
</script>

<template>
    <div class="container">
	<div class="text-container">
	    <input
		v-model="arText"
		dir="rtl"
		lang="ar"
		class="text-input"
		@keydown="handleKey"
		@keypress.prevent="translateKey"
		placeholder="اكتب هنا"
	    >
	</div>
	<Keyboard :active-key="activeKey" />
    </div>
</template>

<style scoped>
    .container {
	margin: 1rem auto;      /* my-4 mx-auto */
	max-width: 85rem;       /* max-w-[85rem] */
	padding-top: 4rem;      /* pt-16 */
    }

    .text-container {
	display: flex;
	justify-content: center;
	align-items: center;
	padding-bottom: 30px;

    }

    .text-input {
	font-family: "ZahirArabic", serif;
	font-size: 2rem;
	padding: 4px;
	border-radius: 4px;
	line-height: 1.6;
    }

</style>
