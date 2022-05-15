<template>
    <div>
        <el-row :gutter="10">
            <el-col :span="6">
                <el-card>
                    <!-- 第一个Card -->
                    <Detail title="总销售额" count="￥ 126560">
                        <template slot="charts">
                            <span>周同比&nbsp;56.67% <svg t="1652348916773" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="4380" width="16" height="16"><path d="M255.998 488.11h167.253l0.909 322.563h166.4v-322.56l167.253 0.455L506.819 165.61l-250.821 322.5z" fill="#d81e06" p-id="4381"></path></svg></span>
                            <span>日同比&nbsp;19.96% <svg t="1652349773635" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="5475" width="16" height="16"><path d="M697.359638 689.069835l-166.241208 170.145122c-5.335519 5.459339-11.773138 8.191566-19.316951 8.191566-7.54279 0-13.982456-2.732228-19.315928-8.191566l-166.241208-170.145122c-8.002254-8.18645-10.251481-19.718087-5.92085-30.417777 4.329609-10.697643 13.921058-17.203824 25.234731-17.203824l134.206608 0 0-452.229708c0-17.838274 14.397918-32.296567 32.236192-32.296567 17.833157 0 32.232099 14.458293 32.232099 32.296567l0 452.229708L678.04371 641.448234c11.31572 0 20.908192 6.506181 25.236777 17.203824C707.610096 669.351748 705.358823 680.883385 697.359638 689.069835z" p-id="5476" fill="#1afa29"></path></svg></span>
                        </template>
                        <template slot="footer">
                            <span>日销售额￥ 12423</span>
                        </template>
                    </Detail>

                </el-card>
            </el-col>
            <el-col :span="6">
                <el-card>
                    <Detail title="访问量" count="88460">
                        <template slot="charts">
                            <lineCharts/>
                        </template>
                        <template slot="footer">
                            <span>日访问量 1234</span>
                        </template>
                    </Detail>
                </el-card>
            </el-col>
            <el-col :span="6">
                <el-card>
                    <Detail title="支付笔数" count="88460">
                        <template slot="charts">
                            <barCharts></barCharts>
                        </template>
                        <template slot="footer">
                            <span>转换率64%</span>
                        </template>
                    </Detail>
                </el-card>
            </el-col>
            <el-col :span="6">
                <el-card>
                    <Detail title="运营活动效果" count="78%">
                        <template slot="charts">
                            <progressCharts></progressCharts>
                        </template>
                        <template slot="footer">
                            <span>周同比&nbsp;&nbsp;56% <svg t="1652348916773" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="4380" width="16" height="16"><path d="M255.998 488.11h167.253l0.909 322.563h166.4v-322.56l167.253 0.455L506.819 165.61l-250.821 322.5z" fill="#d81e06" p-id="4381"></path></svg></span>
                            <span>日同比&nbsp;&nbsp;19% <svg t="1652349773635" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="5475" width="16" height="16"><path d="M697.359638 689.069835l-166.241208 170.145122c-5.335519 5.459339-11.773138 8.191566-19.316951 8.191566-7.54279 0-13.982456-2.732228-19.315928-8.191566l-166.241208-170.145122c-8.002254-8.18645-10.251481-19.718087-5.92085-30.417777 4.329609-10.697643 13.921058-17.203824 25.234731-17.203824l134.206608 0 0-452.229708c0-17.838274 14.397918-32.296567 32.236192-32.296567 17.833157 0 32.232099 14.458293 32.232099 32.296567l0 452.229708L678.04371 641.448234c11.31572 0 20.908192 6.506181 25.236777 17.203824C707.610096 669.351748 705.358823 680.883385 697.359638 689.069835z" p-id="5476" fill="#1afa29"></path></svg></span>
                        </template>
                    </Detail>
                </el-card>
            </el-col>
        </el-row>
    </div>
</template>
<script>
import Detail from './Detail'
import lineCharts from './lineChart'
import barCharts from './barCharts'
import progressCharts from './progressCharts'
export default {
    name:'',
    components:{
        Detail,lineCharts,barCharts,progressCharts
    }
}
</script>
<style lang="">
    
</style>