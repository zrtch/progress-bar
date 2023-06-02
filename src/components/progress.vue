<template>
  <div class="container">
    <div class="echarts_body" ref="charts"></div>
    <div class="status">
      <img :src="img" alt="" />
      <div class="title">
        <span>检测状态</span>
        <span :style="{ color: statusColor }">{{ status }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import img1 from '../assets/weizhi@3x.png'
import img2 from '../assets/zhengchang@3x.png'
import img3 from '../assets/baojing@3x.png'
import * as echarts from 'echarts'
import { onMounted, ref } from 'vue'

const props = defineProps(['count'])
const status = ref('')
const statusColor = ref('')
const img = ref(img1)

onMounted(() => {
  draw()
  getprogress()
})

function getprogress() {
  const val = props.count

  const statusMap = {
    0: { status: '未知', img: img1, color: '#A3A3BF' },
    1: { status: '正常', img: img2, color: '#2CA7FF' },
    2: { status: '预警', img: img3, color: '#FF434D' },
  }

  const {
    status: statusValue,
    img: imgValue,
    color: colorValue,
  } = val == 0 ? statusMap[0] : val <= 50 ? statusMap[1] : statusMap[2]

  status.value = statusValue
  img.value = imgValue
  statusColor.value = colorValue
}

const charts = ref(null)

function draw() {
  let myChart = echarts.init(charts.value)
  const percent = props.count
  let option

  option = {
    title: {
      text: `{a|${percent}} {b|%}`,
      left: 'center',
      top: '38%',
      textStyle: {
        rich: {
          a: {
            color: 'rgb(141, 100, 251)',
            fontSize: 40,
            fontFamily: 'DINCond',
          },
          b: {
            color: 'rgb(141, 100, 251)',
            fontSize: 24,
            fontFamily: 'DINCond',
          },
        },
      },
      subtext: '异常分数',
      subtextStyle: {
        color: 'rgb(88, 85, 154)',
        fontSize: 18,
      },
    },
    polar: {
      radius: ['50%', '60%'],
      center: ['50%', '50%'],
    },
    // 极坐标角度轴
    angleAxis: {
      min: 0,
      max: 100,
      clockwise: true,
      show: false, // 隐藏刻度线
    },
    // 极坐标径向轴
    radiusAxis: {
      type: 'category',
      // 隐藏极坐标轴线
      show: false, // 隐藏刻度线
      axisLine: {
        show: false,
      },
      axisTick: {
        show: false,
      },
    },
    tooltip: {
      show: true,
      formatter: (val) => {
        return `${val.seriesName}: ${val.data}%`
      },
      backgroundColor: 'rgba(31, 196, 225, 0.2)',
      borderColor: 'rgba(31, 196, 225, 0.6)',
    },
    series: [
      {
        // 进度条
        type: 'bar',
        name: '占比',
        coordinateSystem: 'polar',
        showBackground: true,
        // 圆角
        roundCap: true,
        itemStyle: {
          color: {
            x: 0,
            y: 0,
            x1: 0,
            y1: 1,
            colorStops: [
              {
                offset: 0,
                color: 'rgba(171, 141, 249,1)',
              },
              {
                offset: 0.5,
                color: 'rgba(171, 141, 249,0.6)',
              },
              {
                offset: 1,
                color: 'rgba(171, 141, 249,1)',
              },
            ],
          },
        },
        data: [percent],
      },
    ],
  }

  option && myChart.setOption(option)
}
</script>

<style lang="less" scoped>
@font-face {
  font-family: DINCond;
  src: url('../assets/DINCond-Black.otf');
}
.container {
  height: 440px;
  width: 340px;
  // margin-right: 40px;
  border-radius: 10px;
  background: rgba(204, 205, 210, 0.1);
  .echarts_body {
    width: 340px;
    height: 340px;
  }
  .status {
    height: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
    img {
      width: 50px;
      height: 50px;
    }
    .title {
      margin-left: 10px;
      span {
        display: block;
        text-align: left;
        font-size: 14px;
      }
    }
  }
  .slide {
    height: 40px;
    .slider {
      width: 100%;
      height: 20px;
    }
    .slider::-webkit-slider-thumb {
      -webkit-appearance: none;
      background: #0078d4;
      height: 20px;
      width: 20px;
      border-radius: 50%;
      margin-top: -8px;
    }
  }
}
</style>
