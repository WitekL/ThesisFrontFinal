<script>
import { Line } from 'vue-chartjs'

  export default {
    name: 'Chart',
    extends: Line,
    props: ["chartData"],

    data() {
      return {

      }
    },

    watch: {
      chartData: function() {
        let chart = this.$data._chart;

        chart.data.labels = this.chartData.freq

        chart.data.datasets[0].data = this.chartData["re1"]
        chart.data.datasets[1].data = this.chartData["im1"]
        chart.data.datasets[2].data = this.chartData["re2"]
        chart.data.datasets[3].data = this.chartData["im2"]
        chart.data.datasets[4].data = this.chartData["re3"]
        chart.data.datasets[5].data = this.chartData["im3"]
        chart.data.datasets[6].data = this.chartData["re4"]
        chart.data.datasets[7].data = this.chartData["im4"]

        chart.update();
      }
    },

    mounted() {
      this.renderChart({
        labels: [],
        datasets: [
          {
            label: 'Re11',
            data: [],
            fill: false,
            borderColor: 'red'
          },
          {
            label: 'Im11',
            data: [],
            fill: false,
            borderColor: 'red',
            borderDash: [5, 5]
          },
          {
            label: 'Re21',
            data: [],
            fill: false,
            borderColor: 'blue'
          },
          {
            label: 'Im21',
            data: [],
            fill: false,
            borderColor: 'blue',
            borderDash: [5, 5]
          },
          {
            label: 'Re12',
            data: [],
            fill: false,
            borderColor: 'green'
          },
          {
            label: 'Im12',
            data: [],
            fill: false,
            borderColor: 'green',
            borderDash: [5, 5]
          },
          {
            label: 'Re22',
            data: [],
            fill: false,
            borderColor: 'orange'
          },
          {
            label: 'Im22',
            data: [],
            fill: false,
            borderColor: 'orange',
            borderDash: [5, 5]
          }
        ],
          scales: {
            yAxes: [{
              ticks: { 
                suggestedMin: -0.5,
                suggestedMax: 1
              }
            }]
          }
      })
    }
  }
</script>

