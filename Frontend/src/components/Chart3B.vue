<template>
	<div class="chart">
		<div id="main" style="width: 100%; height: 100%;">
			<div id="loading"
				style="width: 100%; height: 100%; display: flex; justify-content: center; align-items: center;">
				<img src="../assets/loading.gif" width="10%" height="auto" />
			</div>
		</div>
	</div>
</template>

<script>
	import $ from 'jquery';
	export default {
		name: 'Chart',
		data() {
			return {
				data: null
			}
		},
		methods: {
			fetch() {
				var self = this;
				$.ajax({
					url: "http://172.26.129.23/s3/b",
					type: "GET",
					dataType: "json",
					success: function(data) {
						document.getElementById('loading').style.display = 'none';
						console.log(data);
						self.data = data;
						self.initChart();
						console.log(self.data);
					},
					error: function() {
						console.log("error");
					}
				})
			},
			initChart() {
				var chart = this.$echarts.init(document.getElementById("main"));
				var option = {
					title: {
						text: 'Average Sentiment Score v.s. Proportion Of Non-English Tweets Average Sentiment Score Over The Past Month',
						subtext: 'At City Level',
						x: 'center',
						y: 'top'
					},
					tooltip: {
						showDelay: 0,
						formatter: function(params) {
							if (params.value.length > 1) {
								return params.seriesName + ' :<br/>' + 'Average Sentiment Score: ' +
									params.value[0] + '<br/>' + 'Proportion: ' +
									params.value[1];
							}
						}
					},
					legend: {
						data: ["Adelaide",
							"Brisbane",
							"Canberra",
							"Melbourne",
							"Perth",
							"Sydney"
						],
						left: 'center',
						bottom: 10
					},
					xAxis: {
						name: 'Average\nSentiment\nScore',
					},
					yAxis: {
						name: 'Proportion'
					},
					series: [{
							name: 'Adelaide',
							symbolSize: 10,
							data: [
								[this.data.afinn_avg[0], this.data.nonenglish_avg[0]]
							],
							type: 'scatter'
						},
						{
							name: 'Brisbane',
							symbolSize: 10,
							data: [
								[this.data.afinn_avg[1], this.data.nonenglish_avg[1]]
							],
							type: 'scatter'
						},
						{
							name: 'Canberra',
							symbolSize: 10,
							data: [
								[this.data.afinn_avg[2], this.data.nonenglish_avg[2]]
							],
							type: 'scatter'
						},
						{
							name: 'Melbourne',
							symbolSize: 10,
							data: [
								[this.data.afinn_avg[3], this.data.nonenglish_avg[3]]
							],
							type: 'scatter'
						},
						{
							name: 'Perth',
							symbolSize: 10,
							data: [
								[this.data.afinn_avg[4], this.data.nonenglish_avg[4]]
							],
							type: 'scatter'
						},
						{
							name: 'Sydney',
							symbolSize: 10,
							data: [
								[this.data.afinn_avg[5], this.data.nonenglish_avg[5]]
							],
							type: 'scatter'
						}
					]
				};
				chart.setOption(option);
			}
		},
		mounted() {
			this.fetch();
			// this.initChart();
		}
	}
</script>

<style>
	.chart {
		height: 100%;
		width: 100%;
	}
</style>
