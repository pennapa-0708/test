<template>
  <div class="chart">
    <linechart
      :chart-data="datacollection"
      :height="300"
      :options="options"
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
          fontColor: '#7C818D',
          fontSize: 14,
        },
        tooltips: {
          mode: 'index',
          backgroundColor: 'rgba(255, 255, 255, 0.9)',
          borderColor: '#E9ECF3',
          titleFontColor: '#7C818D',
          titleFontSize: 12,
          bodyFontColor: '#272E39',
          displayColors: false,
          cornerRadius: 2,
          caretSize: 0,
          caretPadding: 15,
          xPadding: 15,
          yPadding: 15,
          position: 'nearest',
        },
        elements: {
          line: {
            tension: 0,
            borderWidth: 1.5,
            borderColor: '#5847D7',
            backgroundColor: '#5847D710',
          },
          point: {
            pointStyle: 'circle',
            backgroundColor: '#5847D7',
            borderColor: '#5847D710',
            radius: 0,
            hoverBorderWidth: 15,
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
            label: 'Total amount ',
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
  font-family: 'Sarabun Regular';
}
</style>
