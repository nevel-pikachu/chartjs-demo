<template>
  <canvas class="container" ref="myChart" width="1440" height="300"></canvas>
</template>

<script>
import moment from 'moment'
import data from '../data.json'
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
    let barSumValueData = [];
    const datasets = data.map(item => {
      if (item.type === 'line') {
        item.data = item.datasets.map(el => el.value);
        item.borderColor = '#33C963';
        item.pointRadius = 5;
        item.pointBackgroundColor = item.color;
        item.borderDashOffset = 0.2;
        item.tension = 1;
        item.lineTension = 0.4;
      } else if (item.type === 'bar') {
        if (!barSumValueData.length) {
          item.data = item.datasets.map(el => el.value);
        } else {
          item.data = item.datasets.map((el, index) => el.value + barSumValueData[index]);
        }
        barSumValueData = item.data;
        item.barPercentage = 0.5;
        item.backgroundColor = item.color
      }
      return item;
    })
    const myChart = this.$refs.myChart;
    const million = Math.pow(10, 6);
    const billion = Math.pow(10, 9);

    // column amount in chart grid
    const capacity = 5;

    // fake data for bar chart and line chart
    // const firstBarData = Array.from({ length: capacity }, () => Math.floor(Math.random() * 5 * million + 1000000));
    // const secondBarData = firstBarData.map(item => item + Math.floor(Math.random() * 5 * million));
    // const lineData = Array.from({ length: capacity }, () => Math.floor(Math.random() * 10 * million));
    // console.log(data[0].datasets.map(item => item.key));
    // const today = new Date("2023-01-20");

    // chart config
    new ChartJS(myChart, {
      type: "bar",
      data: {
        // label x-axis
        labels: data[0].datasets.map(item => item.key),
        datasets: datasets
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