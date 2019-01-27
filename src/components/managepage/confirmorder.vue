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
      <!--机票信息-->
      <el-card class="box-card" style="width:65%">
        <div slot="header" class="clearfix">
          <span>机票信息</span>
          <el-button style="float: right; padding: 3px 0" type="text" @click="clickopendialog">更改航班</el-button>
        </div>
        <div>
          <el-table :data="tableData3" style="width:100%">
            <el-table-column prop="tag" label>
              <template slot-scope="scope">
                <el-tag
                  :type="scope.row.tag === '去程' ? 'primary' : 'success'"
                  disable-transitions
                >{{scope.row.tag}}</el-tag>
              </template>
            </el-table-column>
            <el-table-column prop="date" label="起飞时间"></el-table-column>
            <el-table-column prop="province" label="起飞地点"></el-table-column>
            <el-table-column prop="city" label="到达时间"></el-table-column>
            <el-table-column prop="city" label="到达地点"></el-table-column>
            <el-table-column prop="zip" label="航班号"></el-table-column>
            <el-table-column prop="zip" label="座位类型"></el-table-column>
            <el-table-column label>
              <template slot-scope="scope">
                <el-popover
                  placement="top-start"
                  title="退改签规定"
                  width="200"
                  trigger="hover"
                  content="起飞前2小时扣除80%票价;起飞前2小时后扣除100%票价"
                >
                  <el-button slot="reference">退改签</el-button>
                </el-popover>
              </template>
            </el-table-column>
          </el-table>
        </div>
        <div class="price">
          <span class="price-subtotal">小计:1000</span>
          <br>
          <span class="price-tax">燃油基建:50</span>
        </div>
      </el-card>

      <p style="height:5px;"></p>

       <!--火车票信息-->
      <el-card class="box-card" style="width:65%">
        <div slot="header" class="clearfix">
          <span>火车票信息</span>
          <el-button style="float: right; padding: 3px 0" type="text" @click="clickopendialog">更改火车票</el-button>
        </div>
        <div>
          <el-table :data="tableData3" style="width:100%">
            <el-table-column prop="tag" label>
              <template slot-scope="scope">
                <el-tag
                  :type="scope.row.tag === '去程' ? 'primary' : 'success'"
                  disable-transitions
                >{{scope.row.tag}}</el-tag>
              </template>
            </el-table-column>
            <el-table-column prop="date" label="出发时间"></el-table-column>
            <el-table-column prop="province" label="出发地"></el-table-column>
            <el-table-column prop="city" label="到达时间"></el-table-column>
            <el-table-column prop="city" label="目的地"></el-table-column>
            <el-table-column prop="zip" label="车次"></el-table-column>
            <el-table-column prop="zip" label="座位类型"></el-table-column>
            <el-table-column label>
              <template slot-scope="scope">
                <el-popover
                  placement="top-start"
                  title="退改签规定"
                  width="200"
                  trigger="hover"
                  content="起飞前2小时扣除80%票价;起飞前2小时后扣除100%票价"
                >
                  <el-button slot="reference">退改签</el-button>
                </el-popover>
              </template>
            </el-table-column>
          </el-table>
        </div>
        <div class="price">
          <span class="price-subtotal">小计:1000</span>
        </div>
      </el-card>

      <p style="height:5px;"></p>

      <!--酒店信息-->
      <el-card class="box-card" style="width:65%">
        <div slot="header" class="clearfix">
          <span>酒店信息</span>
          <el-button style="float: right; padding: 3px 0" type="text" @click="clickopenhdialog">更改酒店</el-button>
        </div>
        <div>
          <el-table :data="tableData2" style="width:100%">
            <el-table-column prop="city" label="酒店名称"></el-table-column>
            <el-table-column prop="address" label="酒店地址"></el-table-column>
            <el-table-column prop="date" label="入住时间"></el-table-column>
            <el-table-column prop="date1" label="离店时间"></el-table-column>
            <el-table-column prop="fightno" label="房型"></el-table-column>
            <el-table-column prop="count" label="间数"></el-table-column>
            <el-table-column label>
              <template slot-scope="scope">
                <el-popover
                  placement="top-start"
                  title="酒店规定"
                  width="200"
                  trigger="hover"
                  content="起飞前2小时扣除80%票价;起飞前2小时后扣除100%票价"
                >
                  <el-button slot="reference">酒店规定</el-button>
                </el-popover>
              </template>
            </el-table-column>
          </el-table>
        </div>
        <div class="price">
          <span class="price-subtotal">小计:1000</span>
          <br>
          <span class="price-tax">房间数:3</span>
        </div>
      </el-card>

      <!--出行人信息-->
      <p style="height:5px;"></p>
      <el-card class="box-card" style="width:65%">
        <div slot="header" class="clearfix">
          <span>出行人信息</span>
        </div>
        <div>
          <el-table :data="tableData3" style="width:100%">
            <el-table-column prop="province" label="姓名"></el-table-column>
            <el-table-column prop="zip" label="身份证号"></el-table-column>
            <el-table-column prop="city" label="电话"></el-table-column>
            <el-table-column prop="zip" label="票号"></el-table-column>
          </el-table>
        </div>
        <div class="price">
          <strong class="price-subtotal">总计:1000</strong>
          <br>
          <span class="price-tax">人数:3</span>
        </div>
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
        <el-table-column property="date" label="出发时间"></el-table-column>
        <el-table-column property="date1" label="返回时间"></el-table-column>
        <el-table-column property="city" label="往返城市"></el-table-column>
        <el-table-column property="fightno" label="航班号"></el-table-column>
        <el-table-column property="address" label="舱位类型"></el-table-column>
      </el-table>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onModifyQrcode(form.userid)">确 定</el-button>
      </div>
    </el-dialog>
    <!--更改酒店弹窗-->
    <el-dialog title="更改酒店" :visible.sync="dialogHFormVisible">
      <el-table
        ref="singleTable"
        :data="tableData1"
        highlight-current-row
        @current-change="handleCurrentChange"
        style="width: 100%"
      >
        <el-table-column property="city" label="酒店名称"></el-table-column>
        <el-table-column property="address" label="酒店地址"></el-table-column>
        <el-table-column property="date" label="入住时间"></el-table-column>
        <el-table-column property="date1" label="离店时间"></el-table-column>
        <el-table-column property="fightno" label="房间类型"></el-table-column>
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
          zip: 200333,
          tag: "去程"
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          province: "上海",
          city: "普陀区",
          address: "上海市普陀区金沙江路 1518 弄",
          zip: 200333,
          tag: "返程"
        }
      ],
      tableData: [
        {
          date: "2016-05-02 09:00-19:00",
          date1: "2016-05-02 10:00-13:00",
          city: "北京首都-上海虹桥-北京首都",
          fightno: "CA2356/CA3467",
          address: "经济舱"
        },
        {
          date: "2016-05-02 09:00-19:00",
          date1: "2016-05-02 10:00-13:00",
          city: "北京首都-上海虹桥-北京首都",
          fightno: "CA2356/CA3467",
          address: "经济舱"
        },
        {
          date: "2016-05-02 09:00-19:00",
          date1: "2016-05-02 10:00-13:00",
          city: "北京首都-上海虹桥-北京首都",
          fightno: "CA2356/CA3467",
          address: "经济舱"
        },
        {
          date: "2016-05-02 09:00-19:00",
          date1: "2016-05-02 10:00-13:00",
          city: "北京首都-上海虹桥-北京首都",
          fightno: "CA2356/CA3467",
          address: "经济舱"
        }
      ],
      tableData1: [
        {
          date: "2016-05-02",
          date1: "2016-05-04",
          city: "北京城市酒店",
          fightno: "双人标间",
          address: "东四十条建设街15号"
        },
        {
          date: "2016-05-02",
          date1: "2016-05-04",
          city: "北京城市酒店",
          fightno: "双人标间",
          address: "东四十条建设街15号"
        },
        {
          date: "2016-05-02",
          date1: "2016-05-04",
          city: "北京城市酒店",
          fightno: "双人标间",
          address: "东四十条建设街15号"
        },
        {
          date: "2016-05-02",
          date1: "2016-05-04",
          city: "北京城市酒店",
          fightno: "双人标间",
          address: "东四十条建设街15号"
        }
      ],
      tableData2: [
        {
          date: "2016-05-02",
          date1: "2016-05-04",
          city: "北京城市酒店",
          fightno: "双人标间",
          address: "东四十条建设街15号",
          count: 1
        }
      ],
      dialogFormVisible: false,
      dialogHFormVisible: false
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
    },
    clickopenhdialog() {
      this.dialogHFormVisible = true;
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
.price {
  padding: 5px;
  float: right;
}
.price-subtotal {
  font-size: 14px;
}
.price-tax {
  font-size: 12px;
}
</style>
