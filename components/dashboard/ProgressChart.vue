<template>
  <apexchart type="radialBar" height="120" width="120" :options="chartOptions" :series="series"></apexchart>
</template>

<script>
  export default {
    data() {
      return {
        series: [70],
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
    }
  }
</script>

<style>

</style>