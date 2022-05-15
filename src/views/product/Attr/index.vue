<template lang="">
    <div>
        <el-card style="margin:20px 0px">
            <CategorySelect @getCategoryId="getCategoryId" :show="!isShowTable"></CategorySelect>
        </el-card>
        <el-card>
            <div v-show="isShowTable">
                <el-button type="primary" icon="el-icon-plus" :disabled="!category3Id" @click="addAttr">添加属性</el-button>
                <!-- 表格：展示平台属性 -->
                <el-table style="width: 100%" border :data="attrList">
                    <el-table-column type="index" label="序号" width="80" align="center">
                    </el-table-column>
                    <el-table-column prop="attrName" label="属性名称" width="150">
                    </el-table-column>
                    <el-table-column prop="prop" label="属性值列表" width="width">
                        <!-- row 是每一个相应平台的属性 -->
                        <template slot-scope="{row,$index}">
                            <el-tag type="success" v-for="(attrValue,index) in row.attrValueList" :key="attrValue.id"
                                style="margin:0px 15px">{{attrValue.valueName}}</el-tag>
                        </template>
                    </el-table-column>
                    <el-table-column prop="prop" label="操作" width="150">
                        <template slot-scope="{row,$index}">
  <div>
    <el-button
      type="warning"
      icon="el-icon-edit"
      size="mini"
      @click="updateAttr(row)"
    ></el-button>
    <el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
  </div>
    </template>
                    </el-table-column>
                </el-table>
            </div>
            <!-- 添加属性|修改属性的结构 -->
            <div v-show="!isShowTable">
                <el-form :inline="true" ref="form" label-width="80px" :model="attrInfo">
                    <el-form-item label="属性名">
                        <el-input placeholder="请输入属性名" v-model="attrInfo.attrName"></el-input>
                    </el-form-item>
                </el-form>
                <el-button type="primary" icon="el-icon-plus" @click="addAttrValue" :disabled="!attrInfo.attrName">添加属性值</el-button>
                <el-button @click="isShowTable=true">取消</el-button>
                <el-table style="width: 100%;margin:20px 0px" border :data="attrInfo.attrValueList">
                    <el-table-column
                        align="center"
                        type="index"
                        label="序号"
                        width="80">
                    </el-table-column>
                    <el-table-column
                        prop="prop"
                        label="属性值名称"
                        width="width">
                        <template slot-scope="{row,$index}">
                            <!-- 这里的结构需要用到span和input进行来回的切换 -->
                            <el-input v-model="row.valueName" placeholder="请输入属性值名称" size="mini" v-if="row.flag" 
                            @blur="toLook(row)" @keyup.enter.native="toLook(row)" :ref="$index"></el-input>
                            <span v-else  @click="toEdit(row,$index)" style="display:block">{{row.valueName}}</span>
                        </template>
                    </el-table-column>
                    <el-table-column
                        prop="prop"
                        label="操作"
                        width="width">
                        <template slot-scope="{row,$indx}">
                            <!-- 气泡确认框 -->
                            <el-popconfirm :title="`确定删除${row.valueName}吗？`" @onConfirm="deleteAttrValue($index)">
                                <el-button 
                            type="danger"
                            icon="el-icon-delete"
                            size="mini"
                            slot="reference"></el-button>
                            </el-popconfirm>
                        </template>
                    </el-table-column>
                </el-table>
                <el-button type="primary" @click="addOrUpdateAttr" :disabled="attrInfo.attrValueList.length<1">保存</el-button>
                <el-button @click="isShowTable=true">取消</el-button>
            </div>
        </el-card>
    </div>
</template>
<script>
//按需引入lodash当中的深拷贝
import cloneDeep from 'lodash/cloneDeep'
export default {
  name: "Attr",
  data() {
    return {
      category1Id: "",
      category2Id: "",
      category3Id: "",
      //接收平台属性的字段
      attrList: [],
      //这个属性是控制table表格显示与隐藏的
      isShowTable: true,
      //收集新增属性|修改属性使用的
      attrInfo: {
        attrName: "", //属性名
        attrValueList: [
          //属性名中属性值，因为属性值可以是多个，因此是数组,每一个属性值都是一个对象需要attrId、valueName
          
        ],
        categoryId: 0, //category3Id
        categoryLevel: 3, //3级
      },
    };
  },
  methods: {
    //自定义事件的回调
    getCategoryId({ categoryId, level }) {
      //区分三级分类相应的id，以及父组件进行存储
      if (level == 1) {
        this.category1Id = categoryId;
        this.category2Id = "";
        this.category3Id = "";
      } else if (level == 2) {
        this.category2Id = categoryId;
        this.category3Id = "";
      } else {
        //代表三级分类id有了
        this.category3Id = categoryId;
        //发请求获取品牌属性
        this.getAttrList();
      }
    },
    //获取平台属性的数据
    //当用户确定三级分类的数据的时候，可以向服务器发请求获取平台属性进行展示
    async getAttrList() {
      //获取分类的id
      const { category1Id, category2Id, category3Id } = this;
      let result = await this.$API.attr.reqAttrList(
        category1Id,
        category2Id,
        category3Id
      );
      if (result.code == 200) {
        this.attrList = result.data;
      }
    },
    //添加属性值回调
    addAttrValue(){
        //向属性值的数组里面添加元素
        //attrId:是你相应的属性的id，目前而言我们是添加属性的操作，还没有相应的属性的id，目前而言带的id为undefined
        //valueName:相应的属性值的名称
        this.attrInfo.attrValueList.push({
            attrId:this.attrInfo.id, //对于修改某一个属性的时候，可以在已有的属性值基础之上新增属性值（新增属性值的时候，需要把已有的属性的id带上）
            valueName:'',
            flag:true, //给每一个属性添加一个flag标记，用于切换查看模式和编辑模式，好处是每一个属性值可以控制自己的模式切换
            //当前flag属性，响应式数据（数据变化视图跟着变化）
        })
        this.$nextTick(()=>{
            this.$refs[this.attrInfo.attrValueList.length-1].focus()
        })
    },
    //添加属性按钮的回调
    addAttr(){
        //切换table的显示与隐藏
        this.isShowTable = false;
        //清除数据
        //收集三级分类的id
        this.attrInfo = {
            attrName: "", //属性名
            attrValueList: [
             //属性名中属性值，因为属性值可以是多个，因此是数组,每一个属性值都是一个对象需要attrId、valueName  
            ],
            categoryId: this.category3Id, //category3Id
            categoryLevel: 3, //3级
        }
    },
    //修改某一个属性
    updateAttr(row){

        //iShowTable变为false
        this.isShowTable = false;
        //将选中的属性赋值给attrInfo
        //由于数据结构当中存在对象里面嵌套数组，数组里面又嵌套对象，因此需要使用深拷贝解决这类问题
        //深拷贝、浅拷贝很重要
        this.attrInfo = cloneDeep(row);
        //在修改某一个属性的时候，将相应的属性值元素添加上flag标记
        this.attrInfo.attrValueList.forEach(item=>{
            //这样书写也可以给属性值添加flag自动，但是会发现视图不会跟着变化(因为flag不是响应式数据)
            //因为Vue无法探测普通新增的 property，这样书写的属性并非响应式属性(数据变化视图也变化)
            //第一个参数：对象 第二个参数：添加新的响应式属性 第三个参数：新的属性的属性值
            this.$set(item,'flag',false)
        })
    },
    //失去焦点的事件--切换为查看模式，展示span 
    toLook(row){
        //如果属性值为空不能作为新的属性值，需要给用户提示，让它输入一个其他属性值
        if(row.valueName.trim()==''){
            this.$message('请你输入一个正常的属性值')
            return ;
        }
        //新增的属性值不能与已有的属性值重复
        let isRepat = this.attrInfo.attrValueList.some(item=>{
            //需要将row从数组里面判断的时候去重
            //row是最新新增的属性值【数组的最后一项元素】
            //判断的时候，需要把已有的数组当中新增的这个属性值去除
            if(row!==item){
                return row.valueName==item.valueName
            }
        })
        if(isRepat) return;
        //row:当前用户添加的最新的属性值   
        //当前的编辑模式变为查看模式【让input消失，显示span】
        row.flag = false
    },
    //点击span的回调，变为编辑模式
    toEdit(row,index){
        row.flag=true
        //获取input节点，实现自动聚焦
        //需要注意：点击span的时候切换为input变为编辑模式，对于浏览器而言，页面重绘与重排会消耗时间
        //点击span的时候，重绘重排一个input它是需要耗费时间，由此不可能点击span立马获取到input
        //nextTick,当节点渲染完毕后会执行一次
        this.$nextTick(()=>{
            //获取相应的input表单元素实现聚焦
            this.$refs[index].focus()
        })
        
    },
    //气泡确认框确定按钮的回调
    //最新版本的elementUI--2.15.7,目前模板的版本号为2.13 前面的confirm得改为onComfirm事件
    async deleteAttrValue(index){
        //当前删除属性得操作是不需要发请求的
        this.attrInfo.attrValueList.splice(index,1)
        // let result = await this.$API.attr.reqDeleteAttr(row.id)
        // if(result.code==200){
        //     const {categoryId,level}=this;
        //     this.$nextTick(()=>{
        //         this.getAttrList();
        //         console.log(result)
        //     })
        // }
    },
    //保存按钮，进行添加属性或者修改属性的操作
    async addOrUpdateAttr(){
        //整理参数：1.如果属性值为空，则不提交给服务器
        //提交给服务器的数据不应该出现flag字段
        this.attrInfo.attrValueList = this.attrInfo.attrValueList.filter(item=>{
            //过滤属性值为空的
            if(item.valueName.trim()!=''){
                //删除flag属性
                delete item.flag
                return true
            }
        })
        try {
            //发请求
            let result = await this.$API.attr.reqAddOrUpdateAttr(this.attrInfo)
            //展示平台属性的信号量进行切换
            this.isShowTable = true
            //提示信息
            this.$message({type:'success',message:'保存成功'})
            //保存成功之后需要再次获取平台属性进行展示
            this.getAttrList()
        } catch (error) {
            this.$message('保存失败')
        }
    }
  },
};
</script>
<style scoped>
</style>