<template>
  <Layout title="自定义图形">
    <div
      id="custom-theme-chart"
      class="k-line-chart"
      v-bind:style="{backgroundColor: theme === 'light' ? '#FFFFFF' : ''}"/>
  </Layout>
</template>

<script>
  import {dispose, init} from "klinecharts"
  import generatedKLineDataList from "../generatedKLineDataList"
  import Layout from "@/Layout"

  const textColorDark = '#929AA5'
  const gridColorDark = '#292929'
  const axisLineColorDark = '#333333'
  // const crossTextBackgroundColorDark = '#373a40'

  const textColorLight = '#76808F'
  const gridColorLight = '#EFF2F5'
  const axisLineColorLight = '#EFF2F5'
  // const crossTextBackgroundColorLight = '#222222'

  function getThemeOptions (theme) {
    const textColor = theme === 'dark' ? textColorDark : textColorLight
    const gridColor = theme === 'dark' ? gridColorDark : gridColorLight
    const axisLineColor = theme === 'dark' ? axisLineColorDark : axisLineColorLight
    // const crossLineColor = theme === 'dark' ? axisLineColorDark : axisLineColorLight
    // const crossTextBackgroundColor = theme === 'dark' ? crossTextBackgroundColorDark : crossTextBackgroundColorLight
    return {
      grid: {
        horizontal: {
          color: '#EFF2F5',
          style: 'solid'
        },
        vertical: {
          show: false,
          color: gridColor
        }
      },
      candle: {
        type: "area",
        area: {
          lineSize: 2,
          lineColor: '#4678EB',
          fillColor: [{
            offset: 0,
            color: 'rgba(70, 120, 235, 0.01)'
          }, {
            offset: 1,
            color: 'rgba(70, 120, 235, 0.1)'
          }]
        },
        priceMark: {
          high: {
            color: textColor
          },
          low: {
            color: textColor
          },
          last: {
            show: false,
          }
        },
        tooltip: {
          showRule: 'none',
          text: {
            color: textColor
          }
        }
      },
      technicalIndicator: {
        line: {
          size: 1,
          colors: ['rgba(0,0,0,0)', 'rgba(0,0,0,0)', 'rgba(0,0,0,0)']
        },
        tooltip: {
          showRule: 'none',
          text: {
            color: textColor
          }
        }
      },
      xAxis: {
        axisLine: {
          color: axisLineColor
        },
        tickLine: {
          show: false,
          color: axisLineColor,
        },
        tickText: {
          color: '#666666',
          size: 14,
        }
      },
      yAxis: {
        position: 'left',
        type: 'normal',
        inside: true,
        axisLine: {
          color: axisLineColor
        },
        tickLine: {
          color: axisLineColor
        },
        tickText: {
          color: textColor,
          size: 13
        }
      },
      separator: {
        color: axisLineColor
      },
      crosshair: {
        horizontal: {
          line: {
            color: '#333333'
          },
          text: {
            backgroundColor: 'rgba(0,0,0,0.8)',
            size: 14,
            color: '#ffffff',
            paddingLeft: 2,
            paddingRight: 2,
            paddingTop: 4,
            paddingBottom: 4,
            borderRadius: 3
          }
        },
        vertical: {
          line: {
            color: '#333333'
          },
          text: {
            backgroundColor: 'rgba(0,0,0,0.8)',
            size: 14,
            color: '#ffffff',
            paddingLeft: 2,
            paddingRight: 2,
            paddingTop: 4,
            paddingBottom: 4,
            borderRadius: 3
          }
        }
      }
    }
  }

  export default {
    name: 'CustomThemeKLineChart',
    components: {Layout},
    data: function () {
      return {
        theme: 'light'
      }
    },
    mounted: function () {
      this.kLineChart = init('custom-theme-chart')
      this.kLineChart.createTechnicalIndicator('VOL', false)
      this.kLineChart.applyNewData(generatedKLineDataList())
      this.setTheme(this.theme)
    },
    methods: {
      setTheme: function (theme) {
        this.theme = theme
        this.kLineChart.setStyleOptions(getThemeOptions(theme))
      }
    },
    destroyed: function () {
      dispose('custom-theme-chart')
    }
  }
</script>
