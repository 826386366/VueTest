<template>
  <div>
    <el-card class="box-card">
      <h3>活动列表</h3>
      <el-row type='flex' align='middle' :gutter='20'>
        <el-col :span="5">
          已选择0个活动
        </el-col>
        <el-col :span="5">
            <el-select v-model="currentType" placeholder="请选择">
              <el-option v-for="item in options" :value="item"></el-option>
            </el-select>
        </el-col>
        <el-col :span="13">
          <el-button type="danger" :plain='true' @click="removeConfirm">删除</el-button>
          <el-button type="danger" :plain='info'>置顶</el-button>
          <el-button type="danger" :plain='info'>复制</el-button>
        </el-col>
      </el-row>
    </el-card>
    <el-table :data="filteredTableData" style="width: 100%" @selection-change="selectionChange">
      <el-table-column type="selection" width="50"></el-table-column>
      <el-table-column property="title" label="活动名称"></el-table-column>
      <el-table-column property="type" label="活动分类"></el-table-column>
      <el-table-column property="status" label="活动状态"></el-table-column>
      <el-table-column property="readNum" label="浏览数"></el-table-column>
      <el-table-column property="signUpNum" label="报名数"></el-table-column>
      <el-table-column property="auditNum" label="待审核"></el-table-column>
      <el-table-column property="id" label="操作"><el-button>复制</el-button></el-table-column>
    </el-table>
    <el-dialog title="提示"  width="30%" :visible="dialogVisible" :before-close="handleClose">
      <span>这是一段信息</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="handleRemove">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>
<script>
  import axios from 'axios'
  export default {
    name:'List',
    data () {
      return{
        tableData:[],
        dialogVisible:false,
        currentType:'全部',
        options:['全部','测试活动','免费活动','收费活动'],
        selectItems:[]
      }
    },
    computed:{ 
      filteredTableData:function () {
        var type = this.currentType;
          return this.tableData.filter(function (data) {
            if(type == '全部'|| type == '' ){
              return true
            }else{
              return data.type == type
            }
          })
      }
    },
    created() {
      axios.get('/static/json/test.json').then((response)=>{
          this.tableData = response.data.list
      })
    },
    methods:{
      removeConfirm(){
        this.dialogVisible = true;
      },
      selectionChange(val){
        var arr = [];
        val.forEach(item => {
          arr.push(item);
        });
        this.selectItems = arr;
      },
      handleRemove(){
        var tableData = this.tableData;
        this.selectItems.forEach(id=>{
          tableData.forEach(data=>{
            if(id.id == data.id){
              tableData.splice(tableData.indexOf(data),1);
            }
          })
        })
        this.selectItems = [];
        this.dialogVisible = false;
      },
      handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
      }
    }
  }
</script>

<style scoped>

</style>
