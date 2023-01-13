<template>
  <canvas class="container" ref="myChart" width="1440" height="300"></canvas>
</template>

<script>
import moment from 'moment'
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
  LineElement,
  PointElement,
  LineController,
  BarController
} from 'chart.js'

ChartJS.register(CategoryScale, LinearScale, LineController, BarController, BarElement, Title, Tooltip, Legend, LineElement, PointElement)

export default {
  mounted() {
    const myChart = this.$refs.myChart;
    const million = Math.pow(10, 6);
    const billion = Math.pow(10, 9);

    // column amount in chart grid
    const capacity = 16;

    // fake data for bar chart and line chart
    const firstBarData = Array.from({ length: capacity }, () => Math.floor(Math.random() * 5 * million + 1000000));
    const secondBarData = firstBarData.map(item => item + Math.floor(Math.random() * 5 * million));
    const lineData = Array.from({ length: capacity }, () => Math.floor(Math.random() * 10 * million));

    const today = new Date("2023-01-20");

    // chart config
    new ChartJS(myChart, {
      type: "bar",
      data: {
        // label x-axis
        labels: Array.from({ length: capacity }, (item, index) => {
          let nextDay = index === 0 ? today.getDate() : today.getDate() + 1;
          return moment(new Date(today.setDate(nextDay))).format("DD/MM")
        }),
        datasets: [
          {
            type: "line",
            label: "Khối lượng giao dịch",
            data: lineData,
            fill: 1,
            borderColor: '#33C963',
            pointRadius: 5,
            pointBackgroundColor: '#33C963',
            borderDashOffset: 0.2,
            tension: 1,
            lineTension: 0.4
          },
          {
            type: "bar",
            backgroundColor: "#84CAFF",
            label: "Phí giao dịch",
            data: firstBarData,
            barPercentage: 0.5
          },
          {
            type: "bar",
            backgroundColor: "#FFC698",
            label: "Phí giao dịch",
            data: secondBarData,
            barPercentage: 0.5
          },
        ]
      },
      options: {
        // stack for bar chart by vertical
        scales: {
          x: {
            stacked: true,

          },
          y: {
            stacked: true,
            ticks: {
              // label y-axis
              callback: function (value) {
                if (value >= billion) {
                  return value / billion + "B"
                } else if (value >= million) {
                  return value / million + "M"
                }
                return value
              }
            }
          },
        },
      }
    });
  }
}
</script>

<style scoped lang="scss">
.container {
  max-width: 1440px;
  margin: 0 auto;
}
</style>