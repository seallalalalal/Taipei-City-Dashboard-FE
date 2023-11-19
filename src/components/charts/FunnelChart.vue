<script setup>
import { ref } from "vue";

const chartOptions = ref({
	chart: {
		type: "bar",
		height: 350,
	},
	plotOptions: {
		bar: {
			borderRadius: 0,
			horizontal: true,
			barHeight: "80%",
			isFunnel: true,
		},
	},
	dataLabels: {
		enabled: true,
		formatter: function (val, opt) {
			return opt.w.globals.labels[opt.dataPointIndex] + ":  " + val;
		},
		dropShadow: {
			enabled: true,
		},
	},
	title: {
		text: "Recruitment Funnel",
		align: "center",
	},
	xaxis: {
		categories: [
			"Sourced",
			"Screened",
			"Assessed",
			"HR Interview",
			"Technical",
			"Verify",
			"Offered",
			"Hired",
		],
	},
	legend: {
		show: false,
	},
});

const chartHeight = 350;

</script>

<template>
	<div v-if="activeChart === 'FunnelChart'">
		<apexchart
			width="100%"
			:height="chartHeight"
			type="bar"
			:options="chartOptions"
			:series="series"
		></apexchart>
	</div>
</template>
