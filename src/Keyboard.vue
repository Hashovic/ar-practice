<script setup>
    import CommandIcon from '@/icons/CommandIcon.vue';
    import LeftArrow from '@/icons/LeftArrowIcon.vue';
    import RightArrow from '@/icons/RightArrowIcon.vue';
    import UpDownArrow from '@/icons/UpDownArrowIcon.vue';
    import { arabic101Map } from "@/helper.js";

    const props = defineProps({activeKey: String})

    const keyMap = {
	  Backspace: "Backspace",
	  Enter: "Enter",
	  Space: " ",
	  Tab: "Tab",
	  Shift: "Shift",
	  Caps: "CapsLock",
	  Ctrl: "Control",
	  Alt: "Alt",
	  Left: "ArrowLeft",
	  Right: "ArrowRight",
	  UpDown: "ArrowUp",
	  SuperKey: "Meta"
    };

    function isActive(label) {
	if (!props.activeKey) return false;
	const key = keyMap[label] || label;
	return props.activeKey.toLowerCase() === key.toLowerCase();
    }

</script>

<template>
    <div class="kbd">
	<div class="row row-1">
	    <div class="items" :class="{ active: isActive(item) }" v-for="item in ['`', '1', '2', '3', '4', '5', '6', '7', '8', '9', '0', '-', '=']">
		<span class="key-en">{{ item }}</span>
		<span class="key-ar">{{ arabic101Map[item.toLowerCase()] }}</span>
	    </div>
	    <div class="items col-2" :class="{ active: isActive('Backspace') }">Backspace</div>
	</div>
	<div class="row row-2">
	    <div class="items col-2" :class="{ active: isActive('Tab') }">Tab</div>
	    <div class="items" :class="{ active: isActive(item) }" v-for="item in ['Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P', '[', ']']">
		<span class="key-en">{{ item }}</span>
		<span class="key-ar">{{ arabic101Map[item.toLowerCase()] }}</span>
	    </div>
	    <div class="items col-2" :class="{ active: isActive('\\') }">\</div>
	</div>
	<div class="row row-3">
	    <div class="items col-2" :class="{ active: isActive('Caps') }">Caps</div>
	    <div class="items" :class="{ active: isActive(item) }" v-for="item in ['A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L', ';', '\'']">
		<span class="key-en">{{ item }}</span>
		<span class="key-ar">{{ arabic101Map[item.toLowerCase()] }}</span>
	    </div>
	    <div class="items col-2" :class="{ active: isActive('Enter') }">Enter</div>
	</div>
	<div class="row row-4">
	    <div class="items col-3" :class="{ active: isActive('Shift') }">Shift</div>
	    <div class="items" :class="{ active: isActive(item) }" v-for="item in ['Z', 'X', 'C', 'V', 'B', 'N', 'M', ',', '.', '/']">
		<span class="key-en">{{ item }}</span>
		<span class="key-ar">{{ arabic101Map[item.toLowerCase()] }}</span>
	    </div>
	    <div class="items col-3" :class="{ active: isActive('Shift') }">Shift</div>
	</div>
	<div class="row row-5">
	    <div class="items" :class="{ active: isActive(item) }" v-for="item in ['Ctrl', 'Fn']">{{ item }}</div>
	    <div class="items" :class="{ active: isActive('SuperKey') }">
		<CommandIcon />
	    </div>
	    <div class="items" :class="{ active: isActive('Alt') }">Alt</div>
	    <div class="items col-5" :class="{ active: isActive('Space') }">Space</div>
	    <div class="items" :class="{ active: isActive(item) }" v-for="item in ['Alt', 'Ctrl']">{{ item }}</div>
	    <div class="items" :class="{ active: isActive('Left') }">
		<LeftArrow />
	    </div>
	    <div class="items" :class="{ active: isActive('UpDown') }">
		<UpDownArrow />
	    </div>
	    <div class="items" :class="{ active: isActive('Right') }">
		<RightArrow />
	    </div>
	</div>
    </div>
</template>

<style scoped>
    .row {
	display: grid;
	column-gap: 8px;
    }

    .row-1 {grid-template-columns: repeat(15, 1fr)}
    .row-2 {grid-template-columns: repeat(16, 1fr)}
    .row-3 {grid-template-columns: repeat(15, 1fr)}
    .row-4 {grid-template-columns: repeat(16, 1fr)}
    .row-5 {grid-template-columns: repeat(14, 1fr)}

    .col-2 {grid-column-end: span 2}
    .col-3 {grid-column-end: span 3}
    .col-5 {grid-column-end: span 5}
    
    .items {
	border: 2px solid black;
	display: flex;
	justify-content: center;
	align-items: center;
	padding: 25px;
	border-radius: 5px;
	position: relative;        /* parent must be relative for absolute children */
    }

    .active {
	background-color: lightgray;
    }

    /* lg breakpoint ≈ 1024px in Tailwind */
    @media (min-width: 1024px) {
      .container {
	min-width: 85rem;     /* lg:min-w-[85rem] */
      }
    }

    .kbd {
	display: flex;
	flex-direction: column;
	row-gap: 8px;
    }

    /* English letter - top-left corner */
    .key-en {
	position: absolute;
	top: 6px;
	left: 6px;
	font-weight: bold;
    }

    /* Arabic letter - bottom-right corner */
    .key-ar {
	position: absolute;
	bottom: 12px;
	right: 12px;
	font-size: 1.5rem;         /* bigger */
	font-family: "ZahirArabic", serif;
    }

</style>
