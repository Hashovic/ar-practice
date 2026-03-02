<script setup>
    import { arabic101Map } from "@/helper.js";
    const arText = defineModel('arText');
    const activeKey = defineModel('activeKey');

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

	
	// If key is H or Y (alif w/ hamzah)
	if (arabic101Map[key]) {
	    arText.value += arabic101Map[key];
	}
	// If key exists in Arabic map → insert Arabic
	else if (arabic101Map[lower]) {
	    arText.value += arabic101Map[lower];
	}
	// If user is typing Arabic keyboard, keep original
	else {
	    arText.value += key;
	}
    }

</script>
<template>
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
</template>
<style scoped>
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
