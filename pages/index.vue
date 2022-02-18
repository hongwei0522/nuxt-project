<template>
  <div class="index">
    <div class="index-outer">

      <div class="index-head">
        <div class="index-head-logo">
          <img class="index-head-logo-img" src="@/assets/img/logo.png" alt="logo">
        </div>
        <div class="index-head-line"></div>
        <div class="index-head-day">
          <div class="index-head-day-today">Today</div>
          <div class="index-head-day-year">2021年10月18日</div>
        </div>
        <div class="index-head-btn-box">
          <div class="index-head-btn"
            v-for="(head, index) in headData"
            :key="index"
          >
            <div class="index-head-num">{{ head.num }}</div>
            <div class="index-head-desc">{{ head.text }}</div>
          </div>
        </div>
      </div>

      <div class="index-part">
        <div :class="['index-part-btn', {'index-part-btn-select': partNum == 1}]">總部</div>
        <div :class="['index-part-btn', {'index-part-btn-select': partNum == 2}]">二處</div>
        <div :class="['index-part-btn', {'index-part-btn-select': partNum == 3}]">三處</div>
        <div :class="['index-part-btn', {'index-part-btn-select': partNum == 4}]">四處</div>
        <div :class="['index-part-btn', {'index-part-btn-select': partNum == 5}]">五處</div>
        <div :class="['index-part-btn', {'index-part-btn-select': partNum == 6}]">六處</div>
        <div :class="['index-part-btn', {'index-part-btn-select': partNum == 7}]">七處</div>
      </div>

      <div class="index-board">
        <div class="index-board-attend">
          <div class="index-component-title">昨日營運出勤狀態</div>
          <div class="index-component-time"></div>
          <div class="index-component-line"></div>

          <div class="index-board-attend-outbox">
            <div class="index-board-attend-box"
              v-for="(attend, index) in attendData"
              :key="index"
            >
              <div class="index-board-attend-upbox">
                <div class="index-board-attend-text"
                  :style="`color: ${attend.color};`"
                >{{ attend.text }}</div>
                <div class="index-board-attend-data">
                  <div :style="`color: ${attend.color};`"
                    class="index-board-attend-current"
                  >{{ attend.current }}</div>
                  <div class="index-board-attend-sum">/ {{ attend.sum }}</div>
                </div>
              </div>
              <div class="index-board-attend-bar">
                <div :style="`
                  backgroundColor: ${attend.color}; 
                  width: calc(${attend.current}/${attend.sum} * 100%);
                `"
                  class="index-board-attend-bar-outer"
                >
                  <div class="index-board-attend-bar-inner"></div>
                </div>
              </div>
            </div>
          </div>

          <!-- component -->

        </div>

        <div class="index-board-total">
          <div class="index-board-total-box">
            <div class="index-component-title">近15日 總工時</div>
            <div class="index-component-time">2021/10/04~2021/10/18</div>
            <div class="index-component-line"></div>
            <div id="echart1" class="index-board-total-chart"></div>
          </div>

          <div class="index-board-total-box">
            <div class="index-component-title">近15日 總薪資</div>
            <div class="index-component-time">2021/10/04~2021/10/18</div>
            <div class="index-component-line"></div>
            <div id="echart2" class="index-board-total-chart"></div>
          </div>
        </div>

        <div class="index-board-work">
          <div class="index-component-title">每人平均工時</div>
          <div class="index-component-time">2021/10/04~2021/10/18</div>
          <div class="index-component-line"></div>
          <div class="index-board-work-box">
            <div class="index-board-work-row"
              v-for="(work, index) in workData"
              :key="index"
            >
              <div class="index-board-work-day">{{ work.day }}</div>
              <div class="index-board-work-bar">
                <div class="index-board-work-inner-bar"
                  :style="`width: calc(${work.time}/24 * 100%);`"
                ></div>
              </div>
              <div class="index-board-work-num">{{ work.time }}</div>
            </div>
          </div>
        </div>

        <div class="index-board-work">
          <div class="index-component-title">每人平均工時</div>
          <div class="index-component-time">2021/10/04~2021/10/18</div>
          <div class="index-component-line"></div>
          <div class="index-board-work-box">
            <div class="index-board-work-row"
              v-for="(hour, index) in hourData"
              :key="index"
            >
              <div class="index-board-work-day">{{ hour.day }}</div>
              <div class="index-board-work-num">${{ hour.money }}</div>
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>

import * as echarts from 'echarts'
export default {
  head: {
    title: 'Dashboard',
  },
  layout: 'default',
  components: {

  },
  props: {

  },
  data () {
    return {
      headData: [],
      headData1: [
        { num: 3452, text: '綁定人數' },
        { num: 1469, text: '在職人數' },
        { num: 3, text: '新註冊人數' },
        { num: 3, text: '待加保人數' },
        { num: 3, text: '新進人員' },
        { num: 4, text: '待退保' },
        { num: 4, text: '離職人員' }
      ],
      headData2: [
        { num: 2222, text: '綁定人數' },
        { num: 1111, text: '在職人數' },
        { num: 5, text: '新註冊人數' },
        { num: 2, text: '待加保人數' },
        { num: 1, text: '新進人員' },
        { num: 9, text: '待退保' },
        { num: 9, text: '離職人員' }
      ],
      partNum: 1,
      attendData: [
        { text: '已打卡單位', current: 27, sum: 60, color: 'rgb(15, 156, 170)'},
        { text: '未打卡單位', current: 22, sum: 60, color: 'rgb(55, 70, 90)'},
        { text: '排休單位', current: 11, sum: 60, color: 'rgb(231, 172, 48)'},
        { text: '實到人數', current: 120, sum: 148, color: 'rgb(34, 108, 178)'},
        { text: '曠職人數', current: 28, sum: 148, color: 'rgb(229, 84, 5)'},
        { text: '請假人數', current: 0, sum: 148, color: 'rgb(47, 49, 50)'},
      ],
      workData: [
        { day: '10/18', time: 8.378 },
        { day: '10/17', time: 9.378 },
        { day: '10/16', time: 10.378 },
        { day: '10/15', time: 11.378 },
        { day: '10/14', time: 12.378 },
        { day: '10/13', time: 13.378 },
        { day: '10/12', time: 14.378 },
        { day: '10/11', time: 7.378 },
        { day: '10/10', time: 6.378 },
        { day: '10/09', time: 5.378 },
        { day: '10/08', time: 4.378 },
        { day: '10/07', time: 3.378 },
        { day: '10/06', time: 2.378 },
        { day: '10/05', time: 1.378 },
        { day: '10/04', time: 1.378 },
      ],
      hourData: [
        { day: '10/18', money: 197 },
        { day: '10/17', money: 197 },
        { day: '10/16', money: 197 },
        { day: '10/15', money: 197 },
        { day: '10/14', money: 197 },
        { day: '10/13', money: 197 },
        { day: '10/12', money: 197 },
        { day: '10/11', money: 197 },
        { day: '10/10', money: 197 },
        { day: '10/09', money: 197 },
        { day: '10/08', money: 197 },
        { day: '10/07', money: 197 },
        { day: '10/06', money: 197 },
        { day: '10/05', money: 197 },
        { day: '10/04', money: 197 },
      ],
      chart1DataX: ['10/04', '10/05', '10/06', '10/07', '10/08', '10/09', '10/10', '10/11', '10/12', '10/13', '10/14', '10/15', '10/16', '10/17', '10/18'],
      chart1DataY: [1500, 1600, 1300, 1300, 2000, 1300, 2200, 1200, 1300, 1400, 2000, 1400, 1200, 1500, 1200],
      chart2DataX: ['10/04', '10/05', '10/06', '10/07', '10/08', '10/09', '10/10', '10/11', '10/12', '10/13', '10/14', '10/15', '10/16', '10/17', '10/18'],
      chart2DataY: [40, 50, 35, 52, 42, 55, 45, 42, 51, 42, 43, 52, 51, 44, 44],
    }
  },
  mounted () {
    this.headData = this.headData1
    setInterval(() => {
      // head
      if(this.headData == this.headData1) {
        this.headData = this.headData2
      } else {
        this.headData = this.headData1
      }

      // part
      if(this.partNum == 7) {
        this.partNum = 1
      } else {
        this.partNum++
      }
    }, 8000)

    // chart
    let echart1 = this.$echarts.init(document.getElementById('echart1'))
    let echart2 = this.$echarts.init(document.getElementById('echart2'))
    echart1.setOption(this.completeChart(
      this.chart1DataX,
      this.chart1DataY,
      'hr'
    ), true)
    echart2.setOption(this.completeChart(
      this.chart2DataX,
      this.chart2DataY,
      'W'
    ), true)
  },
  destroyed () {
    
  },
  computed: {
    
  },
  methods: {
    completeChart(xData, yData, yText) {
      let chart = {
        grid: {
          top: '10px',
          left: '10px',
          right: '10px',
          bottom: '10px',
          containLabel: true
        },
        xAxis: [
          {
            type: 'category',
            axisLine:{
              lineStyle:{
                color:'rgb(47, 49, 50)'
              }
            },
            // x 座標值的顏色/大小
            axisLabel: {
              textStyle: {
                color: 'rgb(47, 49, 50)',
                fontSize:'12'
              },
            }, 
            data: xData
          }
        ],
        yAxis: [
          {
            type: 'value',
            axisTick: {
              show: false
            },
            axisLine:{
              show: false,
            },
            axisLabel: {
              formatter: `{value} ${yText}`
            }
          }
        ],
        series: [
          {
            data: yData,
            barWidth: 16,
            itemStyle: {
              normal: {
                barBorderRadius: [50, 50, 0 ,0 ],
                color: 'rgb(15, 156, 170)',
              },
              
            },
            type: 'bar'
          }
        ]
      }
      return chart
    },
  }
}
</script>

<style lang="scss" scoped>

.index {
  max-width: 1920px;
  padding: 35px 41px;
  background-color: rgb(241, 242, 243);

  &-outer {
    
  }

  // component

  &-component {

    &-title {
      padding: 12px 0px 0px 20px;
      font-size: 25px;
      font-weight: bold;
      font-weight: bold;
    }

    &-time {
      height: 21px;
      padding: 4px 10px 0px 0px;
      color: rgb(111, 112, 112);
      text-align: right;
    }

    &-line {
      height: 1px;
      margin-top: 7px;
      background-color: rgb(199, 199, 199);
    }
  }
  
  // head

  &-head {
    max-width: 1810px;
    height: 90px;
    display: flex;
    align-items: center;
    padding: 15px 4px 15px 29px;
    box-shadow: 0 4px 12px rgb(0 0 0 / 8%);
    border-radius: 6px;
    background-color: white;

    &-logo {
      width: 313px;
    }

    &-logo-img {
      width: 313px;
    }

    &-line {
      width: 1px;
      height: 85px;
      margin: 0px 28px 0px;
      background-color: rgb(199, 199, 199);
    }

    &-day {
      margin-right: 60px;
  
      &-today {
        font-size: 42px;
      }

      &-year {
        font-size: 24px;
        margin-top: 9px;
      }
    }

    &-btn-box {
      display: flex;
      align-items: center;
    }

    &-btn {
      width: 160px;
      height: 90px;
      margin-right: 11px;
      text-align: center;
      box-shadow: 0 4px 12px rgb(0 0 0 / 8%);
      border-radius: 6px;

      &:nth-child(2) {
        color: rgb(55, 70, 90);
      }

      &:nth-child(3) {
        color: rgb(34, 108, 178);
      }

      &:nth-child(4) {
        color: rgb(34, 108, 178);
      }

      &:nth-child(5) {
        color: rgb(15, 156, 170);
      }

      &:nth-child(6) {
        color: rgb(229, 84, 5);
      }

      &:nth-child(7) {
        color: rgb(111, 112, 112);
      }
    }

    &-num {
      margin-top: 10px;
      font-size: 35px;
    }

    &-desc {
      margin-top: 13px;
      font-size: 16px;
    }
  
  }
  
  // part

  &-part {
    margin-top: 30px;
    display: flex;

    &-btn {
      width: 245px;
      height: 80px;
      line-height: 80px;
      margin-right: 21px;
      text-align: center;
      font-size: 30px;
      font-weight: bold;
      color: rgb(112, 112, 112);
      background-color: white;
      box-shadow: 0 4px 12px rgb(0 0 0 / 8%);
      border-radius: 6px;

      &:last-child {
        margin-right: 0px;
      }
    }

    &-btn-select {
      color: white;
      background-color: rgb(39, 107, 228);
    }
    
  }
  
  // board

  &-board {
    margin-top: 30px;
    display: flex;
    // attend

    &-attend {
      width: 345.5px;
      height: 750px;
      background-color: white;
      box-shadow: 0 4px 12px rgb(0 0 0 / 8%);
      border-radius: 6px;

      &-outbox {
        padding: 30px 20px 33px;
      }

      &-box {
        margin-bottom: 43px;

        &:last-child {
          margin-bottom: 0px;
        }
      }

      &-upbox {
        display: flex;
        justify-content: space-between;
      }

      &-text {
        font-size: 20px;
      }

      &-data {
        display: flex;
        align-items: baseline;
      }

      &-current {
        font-size: 30px;
      }

      &-sum {
        font-size: 18px;
      }

      &-bar {
        height: 20px;
        display: flex;
        align-items: center;
        margin-top: 7px;
        padding: 5px;
        border-radius: 20px;
        background-color: rgb(241, 242, 243);
        box-shadow: 0 4px 12px rgb(0 0 0 / 8%);

        &-outer {
          position: relative;
          height: 20px;
          margin-top: 5px;
          border-radius: 30px;
        }

        &-inner {
          position: absolute;
          top: 5px;
          right: 10px;
          width: 12px;
          height: 12px;
          border-radius: 100%;
          background-color: white;
        }
      }

    }

    // total

    &-total {
      margin-left: 30.5px;

      &-box {
        width: 710px;
        height: 360px;
        background-color: white;
        box-shadow: 0 4px 12px rgb(0 0 0 / 8%);
        border-radius: 6px;

        &:first-child {
          margin-bottom: 30px;
        }
      }

      &-chart {
        margin: 20px 0px 0px 10px;
        width: 690px;
        height: 260px;
      }
    }

    // work

    &-work {
      width: 345.5px;
      height: 750px;
      margin-left: 30px;
      background-color: white;
      box-shadow: 0 4px 12px rgb(0 0 0 / 8%);
      border-radius: 6px;

      &-box {
        padding: 31.5px 22px 31px;
      }

      &-row {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 22px;

        &:last-child {
          margin-bottom: 0px;
        }
      }

      &-day {
        color: rgb(47, 49, 50);
      }

      &-bar {
        width: 164px;
        height: 20px;
        background-color: rgb(225, 230, 236);
      }

      &-inner-bar {
        height: 20px;
        background-color: rgb(217, 162, 81);
      }

      &-num {
        font-size: 20px;
        font-weight: bold;
        color: rgb(47, 49, 50);
      }
    }
    
  }

}

@media( max-width: 500px ){

}

</style>
