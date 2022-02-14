<template>
  <apexchart type="bar" width="100%" height="350" :options="chartOptions" :series="chartSeries"></apexchart>
</template>

<script>
  export default {
    data() {
      return {
        chartSeries: [{
          name: 'Material',
          data: [120, 110, 100, 90, 90, 90, ]
        }, {
          name: 'Jasa',
          data: [70, 65, 60, 60, 50, 50, ]
        }, {
          name: 'Total',
          data: [190, 175, 160, 150, 140, 140, ]
        }],
        chartOptions: {
          chart: {
            type: 'bar',
            width: '100%'
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded'
            },
          },
          dataLabels: {
            enabled: false
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent']
          },
          xaxis: {
            categories: ['PRK', 'SKKI', 'Pengadaan', 'Kontrak', 'Pelaksanaan', 'Pembayaran'],
          },
          yaxis: {
            title: {
              text: 'Rp (juta)'
            }
          },
          fill: {
            opacity: 1
          },
          tooltip: {
            y: {
              formatter: function (val) {
                return "Rp " + val + " juta"
              }
            }
          }
        },
      }
    }
  }
</script>

<style>

</style>