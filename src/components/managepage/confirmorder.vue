<template>
  <div class="table">
    <div class="crumbs">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item>
          <i class="el-icon-lx-cascades"></i> 基础表格
        </el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="container">
      <el-steps :active="1" style="width:65%">
        <el-step title="填单" description="填写行程需求单"></el-step>
        <el-step title="确认" description="确认具体行程"></el-step>
        <el-step title="出行" description="行程制定成功"></el-step>
      </el-steps>
      
      <p style="height:10px;"></p>

      <el-table :data="tableData3" style="width:60%">
        <el-table-column label="行程信息">
          <el-table-column prop="province" label="出发时间" style="width:10%"></el-table-column>
          <el-table-column prop="city" label="出发城市" style="width:10%">></el-table-column>
          <el-table-column prop="address" label="到达时间" style="width:10%">></el-table-column>
          <el-table-column prop="zip" label="到达城市" style="width:10%">></el-table-column>
          <el-table-column prop="zip" label="航班号" style="width:10%">></el-table-column>
          <el-table-column prop="zip" label="仓位" style="width:10%">></el-table-column>
        </el-table-column>
      </el-table>

      <el-table :data="tableData3" style="width:60%">
        <el-table-column label="酒店信息" >
          <el-table-column prop="province" label="酒店名称" style="width:10%">></el-table-column>
          <el-table-column prop="zip" label="酒店地址" style="width:10%">></el-table-column>
          <el-table-column prop="city" label="入住时间" style="width:10%">></el-table-column>
          <el-table-column prop="address" label="离店时间" style="width:10%">></el-table-column>
          <el-table-column prop="zip" label="房型" style="width:10%">></el-table-column>
          <el-table-column prop="zip" label="间数" style="width:10%">></el-table-column>
        </el-table-column>
      </el-table>

       <el-table :data="tableData3" style="width:60%">
        <el-table-column label="出行人信息">
          <el-table-column prop="province" label="出行人姓名" style="width:20%">></el-table-column>
          <el-table-column prop="zip" label="出行人身份证号" style="width:20%">></el-table-column>
          <el-table-column prop="city" label="出行人电话" style="width:20%">></el-table-column>
        </el-table-column>
      </el-table>
       <p style="height:10px;"></p>
      <el-row>
        <el-button type="primary" @click="onSubmit">确认行程</el-button>
      </el-row>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData3: [
        {
          date: "2016-05-03",
          name: "王小虎",
          province: "上海",
          city: "普陀区",
          address: "上海市普陀区金沙江路 1518 弄",
          zip: 200333
        }
      ]
    };
  }
};
</script>
<style scoped>
    .handle-box {
        margin-bottom: 20px;
    }

    .handle-select {
        width: 120px;
    }

    .handle-input {
        width: 300px;
        display: inline-block;
    }
    .del-dialog-cnt{
        font-size: 16px;
        text-align: center
    }
    .table{
        width: 100%;
        font-size: 14px;
    }
    .red{
        color: #ff0000;
    }
</style>
