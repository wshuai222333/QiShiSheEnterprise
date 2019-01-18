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
      <!--行程信息-->
      <el-card class="box-card" style="width:60%" >
        <div slot="header" class="clearfix">
          <span>行程信息</span>
          <el-button style="float: right; padding: 3px 0" type="text" @click="clickopendialog">更改航班</el-button>
        </div>
        <el-table :data="tableData3" style="width:100%" show-summary :summary-method="getSummaries">
          <el-table-column prop="date" label="出发时间"></el-table-column>
          <el-table-column prop="province" label="出发城市"></el-table-column>
          <el-table-column prop="city" label="到达时间"></el-table-column>
          <el-table-column prop="city" label="到达城市"></el-table-column>
          <el-table-column prop="zip" label="航班号"></el-table-column>
          <el-table-column prop="zip" label="座位类型"></el-table-column>
          
        </el-table>
      </el-card>

      <p style="height:5px;"></p>

      <!--酒店信息-->
      <el-card class="box-card" style="width:60%" >
        <div slot="header" class="clearfix">
          <span>酒店信息</span>
          <el-button style="float: right; padding: 3px 0" type="text" @click="clickopendialog">更改酒店</el-button>
        </div>
        <el-table :data="tableData3" style="width:100%" show-summary :summary-method="getSummaries">
          <el-table-column prop="province" label="酒店名称" ></el-table-column>
          <el-table-column prop="zip" label="酒店地址" ></el-table-column>
          <el-table-column prop="city" label="入住时间" ></el-table-column>
          <el-table-column prop="address" label="离店时间" ></el-table-column>
          <el-table-column prop="zip" label="房型" ></el-table-column>
          <el-table-column prop="zip" label="间数" ></el-table-column>
        </el-table>
      </el-card>

      <!--出行人信息-->
      <p style="height:5px;"></p>
      <el-card class="box-card" style="width:60%">
        <div slot="header" class="clearfix">
          <span>出行人信息</span>
        </div>
        <el-table :data="tableData3" style="width:100%" show-summary :summary-method="getSummaries">
          <el-table-column prop="province" label="姓名"></el-table-column>
          <el-table-column prop="zip" label="身份证号" ></el-table-column>
          <el-table-column prop="city" label="电话"></el-table-column>
          <el-table-column prop="zip" label="票号"></el-table-column>
        </el-table>
      </el-card>
      <p style="height:10px;"></p>
      <el-row>
        <el-button type="primary" @click="onSubmit">确认行程</el-button>
      </el-row>
    </div>

    <!--更改航班弹窗-->
    <el-dialog title="更改航班" :visible.sync="dialogFormVisible">
      <el-table
        ref="singleTable"
        :data="tableData"
        highlight-current-row
        @current-change="handleCurrentChange"
        style="width: 100%"
      >
        <el-table-column property="date" label="出发日期" style="width: 20%"></el-table-column>
        <el-table-column property="date1" label="出发到达时间" style="width: 20%"></el-table-column>
        <el-table-column property="name" label="出发到达城市" style="width: 20%"></el-table-column>
        <el-table-column property="fightno" label="航班号" style="width: 20%"></el-table-column>
        <el-table-column property="address" label="舱位类型" style="width: 20%"></el-table-column>
      </el-table>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onModifyQrcode(form.userid)">确 定</el-button>
      </div>
    </el-dialog>
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
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          province: "上海",
          city: "普陀区",
          address: "上海市普陀区金沙江路 1518 弄",
          zip: 200333
        }
      ],
      tableData: [
        {
          date: "2016-05-02",
          date1: "09:00-19:00",
          name: "北京首都-上海虹桥",
          fightno: "CA2356",
          address: "经济舱"
        },
        {
          date: "2016-05-03",
          date1: "09:00-19:00",
          name: "北京南苑-上海虹桥",
          fightno: "CA2356",
          address: "经济舱"
        },
        {
          date: "2016-05-02",
          date1: "09:00-19:00",
          fightno: "CA2356",
          address: "经济舱"
        },
        {
          date: "2016-05-02",
          date1: "09:00-19:00",
          name: "北京首都-上海虹桥",
          fightno: "CA2356",
          address: "经济舱"
        }
      ],
      dialogFormVisible: false
    };
  },
  methods: {
    getSummaries(param) {
      const { columns, data } = param;
      const sums = [];
      columns.forEach((column, index) => {
        if (index === 0) {
          sums[index] = "票面价格";
          return;
        }
        if (index === 1) {
          sums[index] = "1000";
          return;
        }
        if (index === 2) {
          sums[index] = "燃油基建费";
          return;
        }
        if (index === 3) {
          sums[index] = "50";
          return;
        }
        if (index === 4) {
          sums[index] = "总价";
          return;
        }
        if (index === 5) {
          sums[index] = "1050";
          return;
        }
      });
      return sums;
    },
    clickopendialog() {
      this.dialogFormVisible = true;
    }
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
.del-dialog-cnt {
  font-size: 16px;
  text-align: center;
}
.table {
  width: 100%;
  font-size: 14px;
}
.red {
  color: #ff0000;
}
</style>
