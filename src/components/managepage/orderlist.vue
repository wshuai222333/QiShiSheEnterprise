
<template>
  <div class="table">
    <div class="crumbs">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item>
          <i class="el-icon-lx-cascades"></i> 行程列表
        </el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="container">
      <div class="handle-box">
        <!-- <el-input v-model="select_word" placeholder="筛选关键词" class="handle-input mr10"></el-input>
        <el-button type="primary" icon="search" @click="search">搜索</el-button> -->
      </div>
      <el-table :data="tableData" border class="table" ref="multipleTable">
        <el-table-column prop="OrderId" label="订单号"></el-table-column>
        <el-table-column prop="BookingType" label="预定类型" :formatter="formatterbookingtype"></el-table-column>
        <el-table-column prop="DepartDate" label="出发日期"></el-table-column>
        <el-table-column prop="DepartCity" label="出发城市"></el-table-column>
        <el-table-column prop="ArriveDate" label="到达日期"></el-table-column>
        <el-table-column prop="ArriveCity" label="到达城市"></el-table-column>
        <el-table-column prop="TravelWay" label="行程方式" :formatter="formattertravelway"></el-table-column>
        <el-table-column prop="HotelCheckinDate" label="入住日期"></el-table-column>
        <el-table-column prop="HotelCheckoutDate" label="离店日期"></el-table-column>
        <el-table-column prop="HotelType" label="酒店类型" :formatter="formatterhoteltype"></el-table-column>
        <el-table-column prop="HotelLocation" label="期待位置" :formatter="formatterhotellocation"></el-table-column>
        <el-table-column prop="Status" label="订单状态" width="120">
          <template slot-scope="scope">
            <el-tag v-if="scope.row.Status==0">行程生成中</el-tag>
            <el-tag type="danger" v-if="scope.row.Status==1">待用户确认</el-tag>
            <el-tag type="warning" v-if="scope.row.Status==3">行程采购中</el-tag>
            <el-tag type="success" v-if="scope.row.Status==5">已完成</el-tag>
          </template>
        </el-table-column>
        <el-table-column label="操作" width="120" align="center">
          <template slot-scope="scope">
            <el-button type="text" @click="onClickOrderDetail(scope.row)">订单详情</el-button>
          </template>
        </el-table-column>
      </el-table>
      <div class="pagination">
        <el-pagination
          background
          layout="total,prev, pager, next"
          :total="total"
          :current-page="currentPage"
          @current-change="handleCurrentChange"
        ></el-pagination>
      </div>
    </div>

    <el-dialog title="订单详情" :visible.sync="ordertailVisible" width="80%" top="20px" center>
      <div class="container">
        <el-steps :active="2" style="width:100%" v-if="form.status!=5">
          <el-step title="填单" description="填写行程需求单"></el-step>
          <el-step title="确认" description="确认具体行程"></el-step>
          <el-step title="出行" description="行程制定成功"></el-step>
        </el-steps>

        <el-steps :active="3" style="width:100%" v-if="form.status==5">
          <el-step title="填单" description="填写行程需求单"></el-step>
          <el-step title="确认" description="确认具体行程"></el-step>
          <el-step title="出行" description="行程制定成功"></el-step>
        </el-steps>

        <p style="height:10px;"></p>
        <!--机票信息-->
        <el-card class="box-card" style="width:100%">
          <div slot="header" class="clearfix">
            <span>机票信息</span>
            <el-button
              v-if="form.status==1"
              style="float: right; padding: 3px 0"
              type="text"
              @click="openselectair"
            >更改航班</el-button>
          </div>
          <div>
            <el-table :data="tableDataairtop">
              <el-table-column prop="TravelType" label="行程类型" :formatter="formattertraveltype"></el-table-column>
              <el-table-column prop="DepartDate" label="出发时间"></el-table-column>
              <el-table-column prop="ArriveDate" label="返回时间"></el-table-column>
              <el-table-column prop="Citys" label="往返城市"></el-table-column>
              <el-table-column prop="FightNos" label="航班号"></el-table-column>
              <el-table-column prop="SeatType" label="座位类型" :formatter="formatterseattype"></el-table-column>
              <el-table-column label>
                <template slot-scope="scope">
                  <el-popover
                    placement="top-start"
                    title="退改签规定"
                    width="200"
                    trigger="hover"
                    :content="scope.row.AirTicketRules"
                  >
                    <el-button slot="reference">退改签</el-button>
                  </el-popover>
                </template>
              </el-table-column>
            </el-table>
          </div>
          <div class="price">
            <span class="price-subtotal">小计:{{showTicketPrice}}</span>
            <br>
            <span class="price-tax">燃油基建:{{showFuelPrice}}</span>
          </div>
        </el-card>

        <p style="height:5px;"></p>

        <!--火车票信息-->
        <el-card class="box-card" style="width:100%">
          <div slot="header" class="clearfix">
            <span>火车票信息</span>
            <el-button
              v-if="form.status==1"
              style="float: right; padding: 3px 0"
              type="text"
              @click="openselecttrain"
            >更改车次</el-button>
          </div>
          <div>
            <el-table :data="tableDataTrainTop">
              <el-table-column prop="TravelType" label="行程类型" :formatter="formattertraveltype"></el-table-column>
              <el-table-column prop="DepartDate" label="出发时间"></el-table-column>
              <el-table-column prop="ArriveDate" label="返回时间"></el-table-column>
              <el-table-column prop="Citys" label="往返城市"></el-table-column>
              <el-table-column prop="TrainNos" label="车次"></el-table-column>
              <el-table-column prop="SeatType" label="座位类型" :formatter="formattertrainseattype"></el-table-column>

              <el-table-column label>
                <template slot-scope="scope">
                  <el-popover
                    placement="top-start"
                    title="退改签规定"
                    width="200"
                    trigger="hover"
                    :content="scope.row.TrainTicketRules"
                  >
                    <el-button slot="reference">退改签</el-button>
                  </el-popover>
                </template>
              </el-table-column>
            </el-table>
          </div>
          <div class="price">
            <span class="price-subtotal">小计:{{showtrainprice}}</span>
          </div>
        </el-card>

        <p style="height:5px;"></p>

        <!--酒店信息-->
        <el-card class="box-card" style="width:100%">
          <div slot="header" class="clearfix">
            <span>酒店信息</span>
            <span style="font-size: 12px;padding-left:100px">入住时间：{{this.form.hotelCheckinDate}}</span>
            <span style="font-size: 12px;padding-left:100px">离店时间：{{this.form.hotelCheckoutDate}}</span>
            <el-button
              v-if="form.status==1"
              style="float: right; padding: 3px 0"
              type="text"
              @click="openselecthotel"
            >更改酒店</el-button>
          </div>
          <div>
            <el-table :data="tableDataHotelTop" style="width:100%">
              <el-table-column prop="HotelName" label="酒店名称"></el-table-column>
              <el-table-column prop="HotelAddress" label="酒店地址"></el-table-column>
              <el-table-column label>
                <template slot-scope="scope">
                  <el-popover
                    placement="top-start"
                    title="酒店规定"
                    width="200"
                    trigger="hover"
                    :content="scope.row.HotelRules"
                  >
                    <el-button slot="reference">退改签</el-button>
                  </el-popover>
                </template>
              </el-table-column>
            </el-table>
            <el-table :data="tableDataRooms" style="width:100%">
              <el-table-column prop="ApartmentType" label="房间类型" :formatter="formatterroomtype"></el-table-column>
              <el-table-column prop="Apartmentcount" label="房间数量"></el-table-column>
            </el-table>
          </div>
          <div class="price">
            <span class="price-subtotal">小计:{{hotelTotalPrice}}</span>
          </div>
        </el-card>

        <!--出行人信息-->
        <p style="height:5px;"></p>
        <el-card class="box-card" style="width:100%">
          <div slot="header" class="clearfix">
            <span>出行人信息</span>
          </div>
          <div>
            <el-table :data="tableDataPass" style="width:100%">
              <el-table-column prop="PassengerName" label="姓名"></el-table-column>
              <el-table-column prop="PassengerCardNo" label="证件号"></el-table-column>
            </el-table>
          </div>
          <div class="price">
            <strong class="price-subtotal">总计:{{totalprince}}</strong>
          </div>
        </el-card>

        <p style="height:10px;"></p>
        <el-row v-if="form.status==1">
          <el-button type="primary" @click="submitOrderTravel">确认行程</el-button>
        </el-row>
      </div>

      <!--更改航班弹窗-->
      <el-dialog title="更改航班" :visible.sync="airinnerVisible" append-to-body>
        <el-table
          ref="singleTable"
          :data="tableDataair"
          highlight-current-row
          @current-change="handleCurrentChangeAir"
          style="width: 100%"
        >
          <el-table-column prop="TravelType" label="行程类型" :formatter="formattertraveltype"></el-table-column>
          <el-table-column prop="DepartDate" label="出发时间"></el-table-column>
          <el-table-column prop="ArriveDate" label="返回时间"></el-table-column>
          <el-table-column prop="Citys" label="往返城市"></el-table-column>
          <el-table-column prop="FightNos" label="航班号"></el-table-column>
          <el-table-column prop="SeatType" label="座位类型" :formatter="formatterseattype"></el-table-column>
          <el-table-column prop="TicketPrice" label="票面价格"></el-table-column>
          <el-table-column prop="FuelPrice" label="燃油基建费"></el-table-column>
        </el-table>
        <div slot="footer" class="dialog-footer">
          <el-button @click="airinnerVisible = false">取 消</el-button>
          <el-button type="primary" @click="clickSelectAir">确 定</el-button>
        </div>
      </el-dialog>
      <!--更改火车票弹窗-->
      <el-dialog title="更改航班" :visible.sync="traininnerVisible" append-to-body>
        <el-table
          ref="singleTable"
          :data="tableDataTrain"
          highlight-current-row
          @current-change="handleCurrentChangeTrain"
          style="width: 100%"
        >
          <el-table-column prop="TravelType" label="行程类型" :formatter="formattertraveltype"></el-table-column>
          <el-table-column prop="DepartDate" label="出发时间"></el-table-column>
          <el-table-column prop="ArriveDate" label="返回时间"></el-table-column>
          <el-table-column prop="Citys" label="往返城市"></el-table-column>
          <el-table-column prop="TrainNos" label="车次"></el-table-column>
          <el-table-column prop="SeatType" label="座位类型" :formatter="formattertrainseattype"></el-table-column>
          <el-table-column prop="TicketPrice" label="票面价"></el-table-column>
        </el-table>
        <div slot="footer" class="dialog-footer">
          <el-button @click="traininnerVisible = false">取 消</el-button>
          <el-button type="primary" @click="clickSelectTrain">确 定</el-button>
        </div>
      </el-dialog>
      <!--更改酒店弹窗-->
      <el-dialog title="更改航班" :visible.sync="hotelinnerVisible" append-to-body>
        <el-table
          ref="singleTable"
          :data="tableDataHotel"
          highlight-current-row
          @current-change="handleCurrentChangeHotel"
          style="width: 100%"
        >
          <el-table-column prop="HotelName" label="酒店名称"></el-table-column>
          <el-table-column prop="HotelAddress" label="酒店地址"></el-table-column>
          <el-table-column prop="TotalPrice" label="房间总价"></el-table-column>
        </el-table>
        <div slot="footer" class="dialog-footer">
          <el-button @click="hotelinnerVisible = false">取 消</el-button>
          <el-button type="primary" @click="clickSelectHotel">确 定</el-button>
        </div>
      </el-dialog>
    </el-dialog>
  </div>
</template>

<script>
import Service from "../../_common";

export default {
  name: "basetable",
  data() {
    return {
      cur_page: 1,
      is_search: false,
      currentPage: 1,
      user: null,
      total: 0,
      ordertailVisible: false,
      airinnerVisible: false,
      traininnerVisible: false,
      hotelinnerVisible: false,
      form: {
        orderId: "",
        hotelCheckinDate: "",
        hotelCheckoutDate: "",
        status: "",
        totalPrice: ""
      },
      tableData:[],
      tableDataairtop: [],
      showTicketPrice: 0,
      showFuelPrice: 0,
      tableDataair: [],
      aircurrentRow: null,
      tableDataTrainTop: [],
      tableDataTrain: [],
      showtrainprice: 0,
      traincurrentRow: null,
      tableDataHotelTop: [],
      tableDataHotel: [],
      tableDataRooms: [],
      hotelTotalPrice: 0,
      hotelcurrentRow: null,
      tableDataPass: []
    };
  },
  methods: {
    formatterbookingtype(row, column) {
      var msg = "";
      switch (parseInt(row.BookingType)) {
        case 0:
          msg = "机票/火车票";
          break;
        case 1:
          msg = "酒店";
          break;
        case 2:
          msg = "机票/火车票&&酒店";
          break;
        default:
          msg = "未知";
          break;
      }
      return msg;
    },
    formattertravelway(row, column) {
      var msg = "";
      switch (parseInt(row.TravelWay)) {
        case 0:
          msg = "飞机";
          break;
        case 1:
          msg = "火车";
          break;
        default:
          msg = "未知";
          break;
      }
      return msg;
    },
    formattertraveltype(row, column) {
      var msg = "";
      switch (parseInt(row.TravelType)) {
        case 0:
          msg = "单程";
          break;
        case 1:
          msg = "往返";
          break;
        default:
          msg = "未知";
          break;
      }
      return msg;
    },
    formatterhoteltype(row, column) {
      var msg = "";
      switch (parseInt(row.HotelType)) {
        case 0:
          msg = "立即支付";
          break;
        case 1:
          msg = "到店支付";
          break;
        default:
          msg = "未知";
          break;
      }
      return msg;
    },
    formatterhotellocation(row, column) {
      var msg = "";
      switch (parseInt(row.HotelLocation)) {
        case 0:
          msg = "系统默认";
          break;
        case 1:
          msg = "目的地";
          break;
        case 2:
          msg = "机场/车站";
          break;
        default:
          msg = "未知";
          break;
      }
      return msg;
    },
    formatterseattype(row, column) {
      var msg = "";
      switch (parseInt(row.SeatType)) {
        case 0:
          msg = "经济舱";
          break;
        case 1:
          msg = "公务舱";
          break;
        case 2:
          msg = "头等舱";
          break;
        default:
          msg = "未知";
          break;
      }
      return msg;
    },
    formattertrainseattype(row, column) {
      var msg = "";
      switch (parseInt(row.SeatType)) {
        case 0:
          msg = "硬座";
          break;
        case 1:
          msg = "软座";
          break;
        case 2:
          msg = "硬卧";
          break;
        case 3:
          msg = "软卧";
          break;
        case 4:
          msg = "二等座";
          break;
        case 5:
          msg = "一等座";
          break;
        case 6:
          msg = "商务座";
          break;
        default:
          msg = "未知";
          break;
      }
      return msg;
    },
    formatterroomtype(row, column) {
      var msg = "";
      switch (parseInt(row.ApartmentType)) {
        case 0:
          msg = "双人标间";
          break;
        case 1:
          msg = "商务大床";
          break;
        case 2:
          msg = "豪华大床";
          break;
        case 3:
          msg = "豪华套件";
          break;
        default:
          msg = "未知";
          break;
      }
      return msg;
    },
    // 分页导航
    handleCurrentChange(val) {
      this.onQueryClick(val);
    },
    search() {
      this.is_search = true;
    },
    //查询
    onQueryClick: function(pageindex) {
      this.currentPage = pageindex;
      this.$http
        .post(
          "/api/Enterprise/GetDemandOrderList",
          Service.Encrypt.DataEncryption({
            EnterpriseId: this.user.EnterpriseId,
            pageindex: pageindex,
            pagesize: 10
          })
        )
        .then(
          response => {
            if (
              response.data.Data &&
              response.data.Data != null &&
              response.data.Data != undefined
            ) {
              if (response.data.Status == 100) {
                this.total = response.data.Data.TotalItems;
                this.tableData = response.data.Data.Items;
              } else {
                this.$message(response.Message);
              }
            } else {
              this.$message(response.Message);
            }
          },
          error => {
            this.$message(error);
            console.log(error);
          }
        );
    },
    //打开详情
    onClickOrderDetail(order) {
      this.ordertailVisible = true;
      this.form.orderId = order.OrderId;
      this.form.hotelCheckinDate = order.HotelCheckinDate;
      this.form.hotelCheckoutDate = order.HotelCheckoutDate;
      this.form.status = order.Status;
      this.form.totalPrice = order.TotalPrice;

      this.showTicketPrice = 0;
      this.showFuelPrice = 0;
      this.showtrainprice = 0;
      this.hotelTotalPrice = 0;

      this.getGetOrderApartmentList();
      this.getOrderPassengerlist();

      if (this.form.status == 1) {
        this.getselectairlist();
        this.getselecttrainlist();
        this.getselecthotellist();
      } else {
        this.getGetOrderAirTicketList();
        this.getGetOrderTrainTicketList();
        this.getGetOrderHotelList();
      }
    },
    //获取选择航班
    getselectairlist() {
      this.$http
        .post(
          "/api/Boss/GetSelectAirTicketList",
          Service.Encrypt.DataEncryption({
            OrderId: this.form.orderId
          })
        )
        .then(
          response => {
            if (
              response.data.Data &&
              response.data.Data != null &&
              response.data.Data != undefined
            ) {
              if (
                response.data.Status == 100 &&
                response.data.Data.length > 0
              ) {
                this.tableDataair = response.data.Data;
                if (this.tableDataairtop.length < 1) {
                  this.tableDataairtop.push(response.data.Data[0]);
                  this.showTicketPrice = response.data.Data[0].TicketPrice;
                  this.showFuelPrice = response.data.Data[0].FuelPrice;
                } else {
                  this.showTicketPrice = this.tableDataairtop[0].TicketPrice;
                  this.showFuelPrice = this.tableDataairtop[0].FuelPrice;
                }
              } else {
                //this.$message(response.Message);
              }
            } else {
              this.$message(response.Message);
            }
          },
          error => {
            this.$message(error);
            console.log(error);
          }
        );
    },
    //打开选择航班弹窗
    openselectair() {
      this.airinnerVisible = true;
    },
    handleCurrentChangeAir(val) {
      this.aircurrentRow = val;
    },
    clickSelectAir() {
      var list = [];
      list.push(this.aircurrentRow);
      this.tableDataairtop = list;
      this.showTicketPrice = this.aircurrentRow.TicketPrice;
      this.showFuelPrice = this.aircurrentRow.FuelPrice;
      this.airinnerVisible = false;
    },
    //获取火车票
    getselecttrainlist() {
      this.$http
        .post(
          "/api/Boss/GetSelectTrainTicketList",
          Service.Encrypt.DataEncryption({
            OrderId: this.form.orderId
          })
        )
        .then(
          response => {
            if (
              response.data.Data &&
              response.data.Data != null &&
              response.data.Data != undefined
            ) {
              if (
                response.data.Status == 100 &&
                response.data.Data.length > 0
              ) {
                debugger;
                this.tableDataTrain = response.data.Data;
                if (this.tableDataTrainTop.length < 1) {
                  this.tableDataTrainTop.push(response.data.Data[0]);
                  this.showtrainprice = response.data.Data[0].TicketPrice;
                } else {
                  this.showtrainprice = this.tableDataTrainTop.TicketPrice;
                }
              } else {
                //this.$message(response.Message);
              }
            } else {
              this.$message(response.Message);
            }
          },
          error => {
            this.$message(error);
            console.log(error);
          }
        );
    },
    openselecttrain() {
      this.traininnerVisible = true;
    },
    handleCurrentChangeTrain(val) {
      this.traincurrentRow = val;
    },
    clickSelectTrain() {
      var list = [];
      list.push(this.traincurrentRow);
      this.tableDataTrainTop = list;
      this.showtrainprice = this.traincurrentRow.TicketPrice;
      this.traininnerVisible = false;
    },
    //获取选择酒店
    getselecthotellist() {
      this.$http
        .post(
          "/api/Boss/GetSelectHotelList",
          Service.Encrypt.DataEncryption({
            OrderId: this.form.orderId
          })
        )
        .then(
          response => {
            if (
              response.data.Data &&
              response.data.Data != null &&
              response.data.Data != undefined
            ) {
              if (
                response.data.Status == 100 &&
                response.data.Data.length > 0
              ) {
                this.tableDataHotel = response.data.Data;
                debugger;
                if (this.tableDataHotelTop.length < 1) {
                  this.tableDataHotelTop.push(response.data.Data[0]);
                  this.hotelTotalPrice = response.data.Data[0].TotalPrice;
                } else {
                  this.hotelTotalPrice = this.tableDataHotelTop[0].TotalPrice;
                }
              } else {
                // this.$message(response.Message);
              }
            } else {
              this.$message(response.Message);
            }
          },
          error => {
            this.$message(error);
            console.log(error);
          }
        );
    },
    openselecthotel() {
      this.hotelinnerVisible = true;
    },
    handleCurrentChangeHotel(val) {
      this.hotelcurrentRow = val;
    },
    clickSelectHotel() {
      var list = [];
      list.push(this.hotelcurrentRow);
      this.tableDataHotelTop = list;
      this.hotelTotalPrice = this.hotelcurrentRow.TotalPrice;
      this.hotelinnerVisible = false;
    },
    //获取房型数量
    getGetOrderApartmentList() {
      this.$http
        .post(
          "/api/Boss/GetOrderApartmentList",
          Service.Encrypt.DataEncryption({
            OrderId: this.form.orderId
          })
        )
        .then(
          response => {
            if (
              response.data.Data &&
              response.data.Data != null &&
              response.data.Data != undefined
            ) {
              if (response.data.Status == 100) {
                this.tableDataRooms = response.data.Data;
              } else {
                this.$message(response.data.Message);
              }
            } else {
              this.$message(response.data.Message);
            }
          },
          error => {
            this.$message(error);
            console.log(error);
          }
        );
    },
    //获取出行人
    getOrderPassengerlist() {
      this.$http
        .post(
          "/api/Boss/GetOrderPassengerList",
          Service.Encrypt.DataEncryption({
            OrderId: this.form.orderId
          })
        )
        .then(
          response => {
            if (
              response.data.Data &&
              response.data.Data != null &&
              response.data.Data != undefined
            ) {
              if (response.data.Status == 100) {
                this.tableDataPass = response.data.Data;
              } else {
                this.$message(response.data.Message);
              }
            } else {
              this.$message(response.data.Message);
            }
          },
          error => {
            this.$message(error);
            console.log(error);
          }
        );
    },
    //确认行程订单
    submitOrderTravel() {
      
      var selectAirTicketId = 0;
      var selectTrainTicketId = 0;
      var selectHotelId = 0;

      if (this.tableDataairtop.length > 0) {
        selectAirTicketId = this.tableDataairtop[0].SelectAirTicketId;
      }
      if (this.tableDataTrainTop.length > 0) {
        selectTrainTicketId = this.tableDataTrainTop[0].SelectTrainTicketId;
      }
      if (this.tableDataHotelTop.length > 0) {
        selectHotelId = this.tableDataHotelTop[0].SelectHotelId;
      }
      this.$http
        .post(
          "/api/Enterprise/ConfirmOrder",
          Service.Encrypt.DataEncryption({
            OrderId: this.form.orderId,
            SelectAirTicketId: selectAirTicketId,
            SelectTrainTicketId: selectTrainTicketId,
            SelectHotelId: selectHotelId,
            TotalPrice: this.totalprince
          })
        )
        .then(
          response => {
            if (
              response.data.Data &&
              response.data.Data != null &&
              response.data.Data != undefined
            ) {
              if (response.data.Data > 0) {
                this.onQueryClick(1);
                this.$message("确认行程成功，等待采购！");
                this.ordertailVisible = false;
              } else {
                this.$message(response.Message);
              }
            } else {
              this.$message(response.Message);
            }
          },
          error => {
            this.$message(error);
            console.log(error);
          }
        );
    },
    //获取确认机票信息
    getGetOrderAirTicketList() {
      this.$http
        .post(
          "/api/Boss/GetOrderAirTicketList",
          Service.Encrypt.DataEncryption({
            OrderId: this.form.orderId
          })
        )
        .then(
          response => {
            if (
              response.data.Data &&
              response.data.Data != null &&
              response.data.Data != undefined
            ) {
              if (response.data.Status == 100) {
                this.tableDataairtop = response.data.Data;
                this.showTicketPrice = response.data.Data[0].TicketPrice;
                this.showFuelPrice = response.data.Data[0].FuelPrice;
              } else {
                this.$message(response.data.Message);
              }
            } else {
              this.$message(response.data.Message);
            }
          },
          error => {
            this.$message(error);
            console.log(error);
          }
        );
    },
    //获取火车票信息
    getGetOrderTrainTicketList() {
      this.$http
        .post(
          "/api/Boss/GetOrderTrainTicketList",
          Service.Encrypt.DataEncryption({
            OrderId: this.form.orderId
          })
        )
        .then(
          response => {
            if (
              response.data.Data &&
              response.data.Data != null &&
              response.data.Data != undefined
            ) {
              if (response.data.Status == 100) {
                this.tableDataTrainTop = response.data.Data;
                this.showtrainprice = response.data.Data[0].TicketPrice;
              } else {
                this.$message(response.data.Message);
              }
            } else {
              this.$message(response.data.Message);
            }
          },
          error => {
            this.$message(error);
            console.log(error);
          }
        );
    },
    //获取酒店信息
    getGetOrderHotelList() {
      this.$http
        .post(
          "/api/Boss/GetOrderHotelList",
          Service.Encrypt.DataEncryption({
            OrderId: this.form.orderId
          })
        )
        .then(
          response => {
            if (
              response.data.Data &&
              response.data.Data != null &&
              response.data.Data != undefined
            ) {
              if (response.data.Status == 100) {
                this.tableDataHotelTop = response.data.Data;
                this.hotelTotalPrice = response.data.Data[0].TotalPrice;
              } else {
                this.$message(response.data.Message);
              }
            } else {
              this.$message(response.data.Message);
            }
          },
          error => {
            this.$message(error);
            console.log(error);
          }
        );
    }
  },
  mounted() {
    this.user = JSON.parse(localStorage.getItem("ms_username"));
    this.onQueryClick(1);
  },
  activated() {
   this.onQueryClick(1);
 },
 computed: {
    totalprince: function() {
      var total = 0;
      total =
        this.showTicketPrice +
        this.showFuelPrice +
        this.showtrainprice +
        this.hotelTotalPrice;
      return total;
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
.tablerow {
  font-size: 12px;
  color: #909399;
  font-weight: bold;
}
.tablerowtext {
  font-size: 12px;
  line-height: 23px;
  color: #606266;
  padding: 8px 0;
}
</style>
