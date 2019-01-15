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

      chart.data.labels = this.chartData.freq.map(function(x) { return (x / 1000000).toFixed(2); });

      chart.data.datasets[0].data = this.chartData["a11"]
      chart.data.datasets[1].data = this.chartData["a12"]
      chart.data.datasets[2].data = this.chartData["a21"]
      chart.data.datasets[3].data = this.chartData["a22"]

      chart.update();
    }
  },

  mounted() {
    this.renderChart({
      labels: [],
      datasets: [
        {
          label: 's11',
          data: [],
          fill: false,
          borderColor: 'red'
        },
        {
          label: 's12',
          data: [],
          fill: false,
          borderColor: 'orange',
        },
        {
          label: 's21',
          data: [],
          fill: false,
          borderColor: 'blue'
        },
        {
          label: 's22',
          data: [],
          fill: false,
          borderColor: 'green'
        }
      ]
    },
    {
      scales: {
        xAxes: [{
          scaleLabel: {
            display: true,
            labelString: 'Frequency [MHz]',
          }
        }]
      }
    })
  }
}
</script>

