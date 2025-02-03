<template>
	<div class="select_wrap" :class="{ active: isSelActive }">
		<ul class="default_option" @click="(e) => toggleSel(e)">
			<li>
				<div class="option">
					<p>{{ optionActive }}</p>
				</div>
			</li>
		</ul>
		<ul class="select_ul">
			<li v-for="(opt, i) in options">
				<div class="option" @click="pickSel(i)">
					<p>{{ opt }}</p>
				</div>
			</li>
		</ul>
	</div>
</template>

<script setup>
import { ref, defineEmits } from 'vue';
const emit = defineEmits(['filter']);

const options = ref(["All", "Done", "Pending"]);
const optionActive = ref("All");

const isSelActive = ref(false)
function toggleSel(e) {
	isSelActive.value = !isSelActive.value;
}

function pickSel(i) {
	optionActive.value = options.value[i];
	isSelActive.value = false;
	emit('filter', optionActive.value);
}
</script>

<style scoped>
.select_wrap {
	width: 225px;
	margin-left: 0 !important;
	position: relative;
	user-select: none;
	z-index: 1;
}

.select_wrap .default_option {
	background: #fff;
	border-radius: 5px;
	position: relative;
	cursor: pointer;
	border: 2px solid #0b0c0c;
}

.select_wrap .default_option li {
	padding: 10px 20px;
}

.select_wrap .default_option:before {
	content: "";
	position: absolute;
	top: 50%;
	right: 18px;
	width: 10px;
	height: 10px;
	border: 2px solid;
	border-color: transparent transparent #555555 #555555;
	transform: translate(0, -60%) rotate(-45deg);
}

.select_wrap .select_ul {
	position: absolute;
	top: 45px;
	left: 0;
	width: 100%;
	background: #fff;
	border-radius: 5px;
	display: none;
}

.select_wrap .select_ul li {
	padding: 10px 20px;
	cursor: pointer;
}

.select_wrap .select_ul li:first-child:hover {
	border-top-left-radius: 5px;
	border-top-right-radius: 5px;
}

.select_wrap .select_ul li:last-child:hover {
	border-bottom-left-radius: 5px;
	border-bottom-right-radius: 5px;
}

.select_wrap .select_ul li:hover {
	background: #fff4dd;
}

.select_wrap .option {
	display: flex;
	align-items: center;
}

.select_wrap .option .icon {
	background: url('https://i.imgur.com/oEZu0sK.png') no-repeat 0 0;
	width: 32px;
	height: 32px;
	margin-right: 15px;
}

.select_wrap.active .select_ul {
	display: block;
}

.default_option p,
.select_ul li p {
	margin: 5px 0;
}

.select_wrap.active .default_option:before {
	transform: translate(0, -40%) rotate(-225deg);
}
</style>