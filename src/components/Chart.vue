<script>
import { Line } from 'vue-chartjs'

  export default {
    name: 'Chart',
    extends: Line,

    data() {
      return {

      }
    },

    mounted() {
      this.renderChart({
        labels: ['Witek', 'Michau', 'Zdrowik', 'test1', 'test2'],
        datasets: [
          {
            label: 'Jakość programisty',
            data: [100, 60, 20, 10, 15],
            fill: false,
            borderColor: 'red'
          },
          {
            label: 'Test drugiego datasetu',
            data: [80, 40, 20, 10],
            fill: false,
            borderColor: 'blue'
          }
        ]
      },
      {
        responsive: false,
        maintainAspectRatio: false
      })
    }
  }
</script>

