<template>
    <apexchart type="radialBar" height="120" width="120" :options="chartOptions" :series="chartSeries"></apexchart>
</template>

<script>
export default {
    props: ['chartData'],
    data() {
        return {
            chartOptions: {
                chart: {
                    height: 120,
                    type: 'radialBar',
                },
                plotOptions: {
                    radialBar: {
                        hollow: {
                        size: '30%',
                        },
                        dataLabels: {
                        show: false
                        }
                    },
                },
            },
        }
    },
    computed: {
        chartSeries: function() {
            console.log(this.chartData)
            return [parseInt(this.chartData*100)]
        }
    }
  }
</script>

<style>

</style>