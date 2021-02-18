<template>
  <div>
    <linechart
      :chart-data="datacollection"
      :height="300"
      :options="options"
      class="chart"
    ></linechart>
  </div>
</template>
<script>
import linechart from '@/components/linechart'

export default {
  components: {
    linechart,
  },
  data() {
    return {
      datacollection: null,

      options: {
        maintainAspectRatio: false,
        scales: {
          yAxes: [
            {
              ticks: {
                min: 0,
              },
              gridLines: {
                drawBorder: false,
              },
            },
          ],
          xAxes: [
            {
              gridLines: {
                display: false,
              },
            },
          ],
        },
        legend: {
          display: false,
        },
        layout: {
          padding: {
            left: 20,
            right: 20,
            top: 0,
            bottom: 0,
          },
        },
        title: {
          display: true,
          text: 'Amount (THB)',
          position: 'left',
          fontColor:'#7C818D',
          fontSize:14
        },
        tooltips: {
          backgroundColor: '#FFFFFF',
          borderColor: '#E9ECF3',
          titleFontColor: '#7C818D',
          titleFontSize: 12,
          bodyFontColor:'#272E39',
        },
        elements: {
          line: {
            tension: 0,
          },
        },
      },
    }
  },
  mounted() {
    this.fillData()
  },
  methods: {
    fillData() {
      this.datacollection = {
        borderWidth: 100,
        labels: [
          '01/01/2020',
          '03/01/2020',
          '06/01/2020',
          '09/02/2020',
          '12/01/2020',
          '15/01/2020',
          '18/01/2020',
          '21/02/2020',
          '24/01/2020',
          '27/01/2020',
          '31/01/2020',
        ],
        datasets: [
          {
            label: 'Total amount',
            radius: 0,
            hoverBorderWidth: 15,
            borderColor: '#5847D7',
            backgroundColor: '#5847D710',
            data: [
              225000.0,
              250000.0,
              130000.0,
              200000.0,
              200000.0,
              270000.0,
              300000.0,
              260000.0,
              340000.0,
              265000.0,
              250000.0,
            ],
          },
        ],
      }
    },
  },
}
</script>

<style>
.small {
  margin: 150px auto;
}
.chart {
  width: 100%;
  height: 5%;
}
</style>
