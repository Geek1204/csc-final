<template>
  <div style="width: 100%;height: 140px;">
    <div style="width: 350px;height:200px" id="my_charts">

    </div>
  </div>
</template>
<script>
  import eCharts from 'echarts'
  import ajaxUtils from '../../http/ajaxUtils'
  export  default {
    props:["userId"],
    data () {
      return {
        xVal:[],
        yVal:[]
      }
    },
    computed: {
      options() {
        return {
          title: {
            subtext: '用户任务数量波动总览 ( --进入查看任务详情 )',
          },
          color: ['#2bcc63'],
          tooltip : {
            trigger: 'axis',
            axisPointer : {
              type : 'shadow'
            }
          },
          grid: {
            left: '3%',
            right: '4%',
            bottom: '3%',
            containLabel: true
          },
          xAxis : [
            {
              type : 'category',
              data : this.xVal,
              axisTick: {
                alignWithLabel: true
              }
            }
          ],
          yAxis : [
            {
              type : 'value',
              show:false,
            }
          ],
          series : [
            {
              name:'任务数量',
              type:'bar',
              barWidth: '40%',
              data:this.yVal
            }
          ]
        }
      }
    },
    methods: {
      initCharts () {
        if ( this.userId ) {
          let params = {userId:this.userId}
          ajaxUtils.send("/api/user/taskgram",params,result=> {
            if (result.code==200) {
              let source  = result.result
              for (let item in source) {
                this.xVal.push(item)
                this.yVal.push(source[item])
              }
              let dom = document.getElementById("my_charts")
              let charts = eCharts.init(dom)
              charts.setOption(this.options)
            }
          })
        }
      }
    },
    mounted() {
      this.initCharts()
    },
    watch: {
      userId: function () {
        this.initCharts()
      }
    }

  }
</script>
<style>

</style>
