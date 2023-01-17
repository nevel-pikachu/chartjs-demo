<template>
  <div class="container">
    <h1>GIAO DỊCH</h1>
    <canvas id="myChart" width="1440" height="300" />
  </div>
</template>

<script>
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

import data from './data.json'

ChartJS.register(CategoryScale, LinearScale, LineController, BarController, BarElement, Title, Tooltip, Legend, LineElement, PointElement)

export default {
  data () {
    return {
      chartjs: {}
    }
  },
  mounted () {
    let barSumValueData = [];
    const datasets = data.map((item) => {
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
    const myChart = document.getElementById('myChart');
    const million = Math.pow(10, 6);
    const billion = Math.pow(10, 9);

    // chart config
    this.chartjs = new ChartJS(myChart, {
      type: 'bar',
      data: {<template>
  <div class="container">
    <div class="chart">
      <div class="chart__title">
        <span class="title">Giao dịch</span><br>
        <span class="note">Tổng hợp khối lượng giao dịch và phí</span>
      </div>
      <div class="chart__action action">
        <div class="action_btn" @click="handleClick('day')">
          <div :class="{selected: status === 'day'}">Ngày</div>
        </div>
        <div class="action_btn" @click="handleClick('week')">
          <div :class="{selected: status === 'week'}">Tuần</div>
        </div>
        <div class="action_btn" @click="handleClick('month')">
          <div :class="{selected: status === 'month'}">Tháng</div>
        </div>
        <div class="action__export">
          // <img src="../../../../static/assets/images/common/export-icon.svg" alt="export file"/>
          Xuất dữ liệu
        </div>
      </div>
    </div>
    <canvas id="myChart" width="1440" height="300" />
  </div>
</template>

<script>
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

import data from './data.json'

ChartJS.register(CategoryScale, LinearScale, LineController, BarController, BarElement, Title, Tooltip, Legend, LineElement, PointElement)

export default {
  data () {
    return {
      chartjs: {},
      datasets: [],
      status: 'day'
    }
  },
  mounted () {
    const myChart = document.getElementById('myChart');
    this.initChartData();
    const million = Math.pow(10, 6);
    const billion = Math.pow(10, 9);

    // chart config
    this.chartjs = new ChartJS(myChart, {
      type: 'bar',
      data: {
        // label x-axis
        labels: data[0].datasets.map(item => item.key),
        datasets: [...this.datasets]
      },
      options: {
        // stack for bar chart by vertical
        scales: {
          x: {
            stacked: true
          },
          y: {
            stacked: true,
            ticks: {
              // label y-axis
              callback: (value) => {
                if (value >= billion) {
                  return value / billion + 'B'
                } else if (value >= million) {
                  return value / million + 'M'
                }
                return value
              }
            }
          }
        }
      }
    });
  },
  methods: {
    initChartData () {
      this.datasets = data.map((item) => {
        item.data = item.datasets.map(el => el.value);
        if (item.type === 'line') {
          item.borderColor = item.color;
          item.pointRadius = 5;
          item.pointBackgroundColor = item.color;
          item.borderDashOffset = 0.2;
          item.tension = 1;
          item.lineTension = 0.4;
        } else if (item.type === 'bar') {
          item.barPercentage = 0.5;
          item.backgroundColor = item.color
        }
        return item;
      })
    },
    handleClick (value) {
      this.status = value;
    }
  }
}
</script>
<style scoped lang="scss">
  .chart {
    display: flex;
    &__title {
      flex: 2;
      .title {
        font-weight: 500;
        font-size: 20px;
        line-height: 28px;
      }
      .note {
        font-size: 12px;
        line-height: 16px;
        color: #4F4F4F;
      }
    }
    &__action {
      flex: 1;
      display: flex;
      .action_btn {
        flex: 1;
        margin-right: 8px;
        div {
          width: 81px;
          height: 40px;
          text-align: center;
          padding: 8px 0;
          color: #70707B;
          background-color: #FFFFFF;
          border: 1px solid #D1D1D6;
          border-radius: 8px;
          cursor: pointer;
        }
        .selected {
          color: #FAAC2E;
          background-color: #FFFEE8;
          border: 1px solid #FAAC2E;
          border-radius: 8px;
        }
      }
    }
    .action {
      &__export {
        width: 156px;
        height: 40px;
        text-align: center;
        padding: 8px 0;
        color: #70707B;
        background-color: #FFFFFF;
        border: 1px solid #D1D1D6;
        border-radius: 8px;
        cursor: pointer;
      }
    }
  }
</style>
