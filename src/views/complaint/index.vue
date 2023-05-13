<template>
  <el-card>
    <el-row :gutter="20" class="header">
      <el-col :span="7">
        <el-input placeholder="请输入商品名称..." v-model="queryForm.query" clearable></el-input>
      </el-col>
      <el-button type="primary" :icon="Search" @click="initComplaintList">搜索</el-button>
      <el-button type="primary" :icon="DocumentAdd" @click="handleDialogValue()">添加商品</el-button>
    </el-row>
    <el-table :data="tableData" stripe style="width: 100%">
      <el-table-column prop="buyerId" label="买家ID" width="150" fixed/>
      <el-table-column prop="sellerId" label="卖家ID" width="100" />
      <el-table-column prop="content" label="投诉内容" width="200" />
      <el-table-column prop="publishTime" label="投诉时间" width="100" />

      <el-table-column prop="image" label="图片证据" width="150" align="center">
        <template v-slot="scope">
          <img :src="getServerUrl()+'/image/complaint/'+scope.row.pic" width="80" height="80"/>
        </template>
      </el-table-column>
      

      <el-table-column prop="action" label="操作" width="300" fixed="right">
        <template v-slot="scope">
          <el-button type="primary" @click="handleComplaint(scope.row.id,2)">通过</el-button>
          <el-button type="primary" @click="handleComplaint(scope.row.id,0)">不通过</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
        v-model:currentPage="queryForm.pageNum"
        v-model:page-size="queryForm.pageSize"
        :page-sizes="[10, 20, 30, 40,50]"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
    />
  </el-card>
</template>

<script setup>
import { Search ,Delete,DocumentAdd ,Edit} from '@element-plus/icons-vue'
import { ref } from 'vue'
import axios,{ getServerUrl } from "@/util/axios";
import { ElMessage, ElMessageBox } from 'element-plus'

const queryForm=ref({
  query:'',
  pageNum:1,
  pageSize:10
})


const tableData = ref([])


const id=ref(-1)

const initComplaintList=async()=>{
  const res=await axios.post("admin/order/complaintList",queryForm.value)
  tableData.value=res.data.complaintList;
}

initComplaintList();


const handleComplaint=(id,state)=>{
  ElMessageBox.confirm(
      '您确定这条投诉的审核结果吗?',
      '系统提示',
      {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning',
      }
  )
      .then(async() => {
        let res=await axios.post('admin/order/sh_complaint',{id:id,state:state})
        if(res.data.code==0){
          ElMessage({
            type: 'success',
            message: '执行成功',
          })
          initComplaintList();
        }else{
          ElMessage({
            type: 'error',
            message: res.data.msg,
          })
        }

      })
      .catch(() => {

      })
}




const handleSizeChange=(pageSize)=>{
  queryForm.value.pageNum=1;
  queryForm.value.pageSize=pageSize;
  initComplaintList();
}

const handleCurrentChange=(pageNum)=>{
  queryForm.value.pageNum=pageNum;
  initComplaintList();
}



</script>

<style lang="scss" scoped>

.header{
  padding-bottom: 16px;
  box-sizing: border-box;
}

.el-pagination{
  padding-top: 15px;
  box-sizing: border-box;
}


</style>