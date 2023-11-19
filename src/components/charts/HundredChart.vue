<!-- Developed by Taipei Urban Intelligence Center 2023 -->

<script setup>
import { ref } from "vue";

import { computed } from "vue";

const props = defineProps(["chart_config", "activeChart", "series"]);
const count = ref(1);
const isNextClick = ref(undefined);
const iconColor = ref(props.chart_config.color[count.value - 1]);
const parsedSeries = computed(() => {
	const number = props.series[0].data[count.value] * 100;
	return fill1In2d(number);
});

console.log(props.chart_config.color[0]);

function fill1In2d(n) {
	// Create a 10x10 2D array in JavaScript
	const twoDArray = [];

	// Loop to create rows
	for (let i = 0; i < 10; i++) {
		// Initialize an empty row
		const row = [];
		// Loop to create columns in each row
		for (let j = 0; j < 10; j++) {
			// Add an element to the row
			row.push(0);
		}
		// Add the row to the 2D array
		twoDArray.push(row);
	}

	for (let i = 0; i < n; i++) {
		twoDArray[Math.floor(i / 10)][i % 10] = 1;
	}
	return twoDArray;
}

function increaseCount() {
	count.value++;
	isNextClick.value = undefined;
	isNextClick.value = true;
}
function decreaseCount() {
	count.value--;
	isNextClick.value = undefined;
	isNextClick.value = false;
}
function showOrHideButton(isShow) {
	return isShow ? "show" : "hide";
}
function nextOrPrevAnim() {
	console.log({ isNextClick });
	if (isNextClick.value === undefined) {
		console.log("chartcontainer");
		return "chartcontainer";
	}
	console.log("chartcontainer with anim");
	return isNextClick.value
		? "chartcontainer flip-in-ver-left"
		: "chartcontainer flip=in-ver-right";
}
</script>

<style scoped lang="scss">
.hundredchart {
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	gap: 10px;

	* span {
		font-family: var(--font-icon);
		width: 15px;
		height: 15px;
	}
	.chartcontainer {
		display: flex;
		flex-direction: row;
		gap: 10px;
		button {
			img {
				width: 30px;
				height: 30px;
			}
		}
	}
	.flip-in-ver-left {
		animation: flip-in-ver-left 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94)
			both;
	}
	.flip-in-ver-right {
		animation: flip-in-ver-right 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94)
			both;
	}

	@keyframes flip-in-ver-left {
		0% {
			-webkit-transform: rotateY(80deg);
			transform: rotateY(80deg);
			opacity: 0;
		}
		100% {
			-webkit-transform: rotateY(0);
			transform: rotateY(0);
			opacity: 1;
		}
	}

	@keyframes flip-in-ver-right {
		0% {
			-webkit-transform: rotateY(-80deg);
			transform: rotateY(-80deg);
			opacity: 0;
		}
		100% {
			-webkit-transform: rotateY(0);
			transform: rotateY(0);
			opacity: 1;
		}
	}
	.chart {
		.row {
			display: flex;
			flex-direction: column;
			justify-content: center;
			display: flex;
			flex-direction: row;
			justify-content: center;
			gap: 5px;
			.img {
				width: 15px;
				height: 15px;
			}
		}
	}
	.hide {
		visibility: hidden;
	}
	.show {
		visibility: visible;
	}
}
</style>

<template>
	<div v-if="activeChart === 'HundredChart'" class="hundredchart">
		<div>
			每 100 {{ props.chart_config.unit
			}}{{ props.chart_config.categories[count - 1] }}
		</div>
		<div>
			就有 {{ series[0].data[count] * 100 }} {{ props.chart_config.unit
			}}{{ props.chart_config.categories[count] }}
		</div>
		<div :class="nextOrPrevAnim()">
			<button @click="decreaseCount" :class="showOrHideButton(count > 1)">
				<img src="../../assets/images/hundredicon/arrowLeft.svg" />
			</button>
			<div class="chart">
				<div
					v-for="(row, i) in parsedSeries"
					class="row"
					:key="`${row}-${i}`"
				>
					<div v-for="(item, j) in row" :key="`item-${j}`">
						<span
							v-if="item === 1"
							:style="{
								color: props.chart_config.color[count - 1],
							}"
						>
							{{ props.chart_config.icons[count] }}
						</span>
						<span
							v-else
							:style="{
								color: props.chart_config.color[count],
							}"
						>
							{{ props.chart_config.icons[count - 1] }}
						</span>
					</div>
				</div>
			</div>
			<button
				@click="increaseCount"
				:class="
					showOrHideButton(count < props.series[0].data.length - 1)
				"
			>
				<img src="../../assets/images/hundredicon/arrowRight.svg" />
			</button>
		</div>
	</div>
</template>
