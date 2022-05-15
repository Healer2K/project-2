<template lang="">
    <div>
        <div class="header">
            <span class="search-header">搜索用户数</span>
            <svg t="1652347754167" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2562" width="26" height="26"><path d="M536 480v192a16 16 0 0 1-16 16h-16a16 16 0 0 1-16-16V480a16 16 0 0 1 16-16h16a16 16 0 0 1 16 16z m-32-128h16a16 16 0 0 1 16 16v32a16 16 0 0 1-16 16h-16a16 16 0 0 1-16-16v-32a16 16 0 0 1 16-16z m8 448c159.056 0 288-128.944 288-288s-128.944-288-288-288-288 128.944-288 288 128.944 288 288 288z m0 48c-185.568 0-336-150.432-336-336s150.432-336 336-336 336 150.432 336 336-150.432 336-336 336z" p-id="2563"></path></svg>
        </div>
        <div class="main">
            <span class="main-title">13321</span>
            <span class="main-content">17.1</span>
            <svg t="1652348916773" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="4380" width="16" height="16"><path d="M255.998 488.11h167.253l0.909 322.563h166.4v-322.56l167.253 0.455L506.819 165.61l-250.821 322.5z" fill="#d81e06" p-id="4381"></path></svg>
            <svg t="1652349773635" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="5475" width="16" height="16"><path d="M697.359638 689.069835l-166.241208 170.145122c-5.335519 5.459339-11.773138 8.191566-19.316951 8.191566-7.54279 0-13.982456-2.732228-19.315928-8.191566l-166.241208-170.145122c-8.002254-8.18645-10.251481-19.718087-5.92085-30.417777 4.329609-10.697643 13.921058-17.203824 25.234731-17.203824l134.206608 0 0-452.229708c0-17.838274 14.397918-32.296567 32.236192-32.296567 17.833157 0 32.232099 14.458293 32.232099 32.296567l0 452.229708L678.04371 641.448234c11.31572 0 20.908192 6.506181 25.236777 17.203824C707.610096 669.351748 705.358823 680.883385 697.359638 689.069835z" p-id="5476" fill="#1afa29"></path></svg>
        </div>
        <div class="footer">
            <div class="charts" ref="charts">

            </div>
        </div>
    </div>
</template>
<script>
import echarts from "echarts";
export default {
    name:'',
    mounted() {
    //初始化echarts实例
    let lineCharts = echarts.init(this.$refs.charts);
    //配置数据
    lineCharts.setOption({
      xAxis: {
        //隐藏x轴
        show: false,
        type: "category",
      },
      yAxis: {
        //隐藏y轴
        show: false,
      },
      //系列
      series: [
        {
          smooth:true,
          type: "line",
          data: [10, 7, 33, 12, 48, 9,29,10,44],
          //拐点的样式的设置
          itemStyle: {
            opacity: 0,
          },
          //线条的样式
          lineStyle: {
            color: "blue",
          },
          //填充颜色的设置
          areaStyle: {
            color: {
              type: "linear",
              x: 0,
              y: 0,
              x2: 0,
              y2: 1,
              colorStops: [
                {
                  offset: 0,
                  color: "blue", // 0% 处的颜色
                },
                {
                  offset: 1,
                  color: "#fff", // 100% 处的颜色
                },
              ],
              global: false, // 缺省为 false
            },
          },
        },
      ],
      //布局调试
      grid: {
        left: 0,
        top: 0,
        right: 0,
        bottom: 0,
      },
    });
  },
}
</script>
<style scoped>
    .header{
        display: flex;
        align-items: center;
    }
    .search-header{
        margin-right: 30px;
    }
    .main{
        margin:10px 0;
    }
    .main-title{
        margin-right: 30px;
    }
    .main-content{
        margin-right: 10px;
    }
    .charts{
        width: 100%;
        height: 50px;
    }
</style>