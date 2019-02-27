<template>
  <div>
    <div class="crumbs">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item>
          <i class="el-icon-lx-calendar"></i>行程管理
        </el-breadcrumb-item>
        <el-breadcrumb-item>填写行程</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="container">
      <el-steps :active="0" style="width:65%">
        <el-step title="填单" description="填写行程需求单"></el-step>
        <el-step title="确认" description="确认具体行程"></el-step>
        <el-step title="出行" description="行程制定成功"></el-step>
      </el-steps>
      <!--form表单-->
      <div class="form-box">
        <el-form ref="form" :model="form" :rules="rules2" label-width="100px">
          <el-form-item>
            <p style="border-bottom:1px;"></p>
          </el-form-item>
          <el-form-item label="预定类型">
            <el-radio-group v-model="form.bookingtype">
              <el-radio-button label="0" @click.native="onBookingTypeClick(0)">机票/火车票</el-radio-button>
              <el-radio-button label="1" @click.native="onBookingTypeClick(1)">酒店</el-radio-button>
              <el-radio-button label="2" @click.native="onBookingTypeClick(2)">机票/火车票&&酒店</el-radio-button>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="行程城市">
            <el-col :span="8">
              <el-form-item prop="departcity">
                <el-select
                  v-model="form.departcity"
                  filterable
                  :filter-method="querydSearch"
                  placeholder="出发城市/汉字/全拼"
                >
                  <el-option
                    v-for="item in departcitys"
                    :key="item.City_Code"
                    :label="item.City_Name"
                    :value="item.City_Name"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col class="line" :span="2">
              <i class="el-icon-arrow-right"></i>
            </el-col>
            <el-col :span="8">
              <el-form-item prop="arrivecity">
                <el-select
                  v-model="form.arrivecity"
                  filterable
                  :filter-method="queryaSearch"
                  placeholder="到达城市/汉字/全拼"
                >
                  <el-option
                    v-for="item in arrivecitys"
                    :key="item.City_Code"
                    :label="item.City_Name"
                    :value="item.City_Name"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
          </el-form-item>
          <!--行程信息-->
          <div v-show="airshow">
            <el-form-item label="行程类型">
              <el-radio-group v-model="form.traveltype">
                <el-radio-button label="0" @click.native="onTravelTypeClick(0)">单程</el-radio-button>
                <el-radio-button label="1" @click.native="onTravelTypeClick(1)">往返</el-radio-button>
              </el-radio-group>
            </el-form-item>
            <el-form-item label="行程日期">
              <el-row>
                <el-col :span="11">
                  <el-date-picker
                    type="date"
                    placeholder="出发日期"
                    v-model="form.departdate"
                    style="width: 100%;"
                    @change="changedepartdate"
                    value-format="yyyy-MM-dd hh:mm:ss"
                  ></el-date-picker>
                </el-col>
                <el-col class="line" :span="2">
                  <i class="el-icon-minus"></i>
                </el-col>
                <el-col :span="11">
                  <el-select v-model="form.expectdeparttime" placeholder="期望时间段">
                    <el-option
                      v-for="item in expecttraveltimes"
                      :key="item.value"
                      :label="item.label"
                      :value="item.label"
                    ></el-option>
                  </el-select>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="11">
                  <el-form-item prop="arrivedate">
                    <el-date-picker
                      :disabled="form.returndate"
                      type="date"
                      placeholder="返回日期"
                      v-model="form.arrivedate"
                      style="width: 100%;"
                      @change="changearrivedate"
                      value-format="yyyy-MM-dd hh:mm:ss"
                    ></el-date-picker>
                  </el-form-item>
                </el-col>
                <el-col class="line" :span="2">
                  <i class="el-icon-minus"></i>
                </el-col>
                <el-col :span="11">
                  <el-select
                    v-model="form.expectarrivetime"
                    :disabled="form.returndate"
                    placeholder="期望时间段"
                  >
                    <el-option
                      v-for="item in expecttraveltimes"
                      :key="item.value"
                      :label="item.label"
                      :value="item.label"
                    ></el-option>
                  </el-select>
                </el-col>
              </el-row>
            </el-form-item>

            <el-form-item label="期望交通方式">
              <el-radio-group v-model="form.travelway">
                <el-radio-button label="0">飞机</el-radio-button>
                <el-radio-button label="1">火车</el-radio-button>
              </el-radio-group>
            </el-form-item>
            <el-form-item label="其他要求">
              <el-input v-model="form.travelothers" placeholder="头等舱/公务舱/指定航班(选填)"></el-input>
            </el-form-item>

            <el-form-item>
              <p style="border-bottom:solid 1px #ccc;"></p>
            </el-form-item>
            <p style="height:4px;"></p>
          </div>

          <!--酒店信息-->
          <div v-show="hotelshow">
            <el-form-item label="酒店入住日期">
              <el-col :span="11">
                <el-date-picker
                  type="date"
                  placeholder="入住日期"
                  v-model="form.hotelcheckindate"
                  style="width: 100%;"
                  value-format="yyyy-MM-dd hh:mm:ss"
                ></el-date-picker>
              </el-col>
              <el-col class="line" :span="2">-</el-col>
              <el-col :span="11">
                <el-form-item prop="hotelcheckoutdate">
                  <el-date-picker
                    type="date"
                    placeholder="离店日期"
                    v-model="form.hotelcheckoutdate"
                    style="width: 100%;"
                    value-format="yyyy-MM-dd hh:mm:ss"
                  ></el-date-picker>
                </el-form-item>
              </el-col>
            </el-form-item>
            <el-form-item label="酒店类型">
              <el-radio-group v-model="form.hoteltype">
                <el-radio-button label="0">立即支付</el-radio-button>
                <el-radio-button label="1">到店支付</el-radio-button>
              </el-radio-group>
            </el-form-item>
            <el-form-item label="期望酒店位置">
              <el-radio-group v-model="form.hotellocation">
                <el-radio-button
                  label="0"
                  onhotellocationClick
                  @click.native="onhotellocationClick(0)"
                >系统默认</el-radio-button>
                <el-radio-button
                  label="1"
                  onhotellocationClick
                  @click.native="onhotellocationClick(1)"
                >目的地</el-radio-button>
                <el-radio-button
                  label="2"
                  onhotellocationClick
                  @click.native="onhotellocationClick(2)"
                >机场/车站</el-radio-button>
              </el-radio-group>
            </el-form-item>
            <div>
              <el-form-item label="目的地" v-show="destinationshow">
                <el-input v-model="form.destination" placeholder="具体商圈/街区(选填)"></el-input>
              </el-form-item>
            </div>
            <el-form-item label="其他要求">
              <el-input v-model="form.hotelothers" placeholder="其他位置/具体房间要求/指定酒店(选填)"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button
                type="primary"
                icon="el-icon-plus"
                size="small"
                round
                @click="addHDomain"
              >添加房型</el-button>
            </el-form-item>
            <!-- :prop="'domains.' + index + '.value'" -->
            <el-form-item
              v-for="(domain, index) in form.Apartment"
              :label="'房型' + (index+1)"
              :key="domain.key"
              :rules="{
      required: true, message: '房型不能为空', trigger: 'blur'
    }"
            >
              <el-row>
                <el-col :span="5">
                  <el-select v-model="domain.ApartmentType" placeholder="选择房型">
                    <el-option
                      v-for="item in hoteloptions"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                    ></el-option>
                  </el-select>
                </el-col>
                <el-col :span="2" :offset="1">
                  <el-input v-model="domain.Apartmentcount"></el-input>
                </el-col>
                <el-col :span="2">间</el-col>
                <el-col :span="2">
                  <el-button @click.prevent="removeHDomain(domain)">删除</el-button>
                </el-col>
              </el-row>
            </el-form-item>
          </div>

          <!--出行人信息-->
          <el-form-item>
            <el-button
              type="primary"
              icon="el-icon-plus"
              size="small"
              round
              @click="addDomain"
            >添加出行人</el-button>
          </el-form-item>

          <el-form-item
            v-for="(domain, index) in form.Passenger"
            :label="'出行人' + (index+1)"
            :key="domain.key"
            :prop="'Passenger.' + index + '.PassengerName'"
            :rules="[{
      required: true, message: '出行人不能为空', trigger:  ['blur', 'change']
    }]"
          >
            <el-row>
              <el-col :span="6">
                <el-autocomplete
                  class="inline-input"
                  v-model="domain.PassengerName"
                  :fetch-suggestions="querySearch"
                  placeholder="选择出行人"
                  @select="((item)=>{handleSelect(item, domain)})"
                ></el-autocomplete>
                <!-- <el-select v-model="domain.PassengerName" filterable placeholder="选择出行人">
                  <el-option
                    v-for="item in staffoptions"
                    :key="item.StaffId"
                    :label="item.StaffName"
                    :value="item.StaffName"
                  ></el-option>
                </el-select>-->
              </el-col>
              <el-col :span="3" :offset="1">证件号</el-col>
              <el-col :span="8">
                <el-input v-model="domain.PassengerCardNo"></el-input>
              </el-col>
              <el-col :span="4" :offset="1">
                <el-button @click.prevent="removeDomain(domain)">删除</el-button>
              </el-col>
            </el-row>
          </el-form-item>

          <el-form-item>
            <el-button type="primary" @click="onSubmit('form')">提交</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
import Service from "../../_common";

export default {
  name: "baseform",
  data() {
    var validateArrivedate = (rule, value, callback) => {
      if (value) {
        if (value < this.form.departdate) {
          callback(new Error("返回日期不能小于出发日期"));
        } else {
          callback();
        }
      } else {
        callback();
      }
    };
    var validateHotelcheckoutdate = (rule, value, callback) => {
      if (value) {
        if (value < this.form.hotelcheckindate) {
          callback(new Error("离店日期不能小于入住日期"));
        } else {
          callback();
        }
      } else {
        callback();
      }
    };
    return {
      rules2: {
        arrivedate: [{ validator: validateArrivedate, trigger: "change" }],
        hotelcheckoutdate: [
          { validator: validateHotelcheckoutdate, trigger: "change" }
        ],
        departcity: [
          { required: true, message: "请选择出发城市", trigger: "change" }
        ],
        arrivecity: [
          { required: true, message: "请选择到达城市", trigger: "change" }
        ]
      },
      user: null,
      expecttraveltimes: [
        { value: "0", label: "06:00-08:00" },
        { value: "1", label: "08:00-10:00" },
        { value: "3", label: "10:00-12:00" },
        { value: "4", label: "12:00-14:00" },
        { value: "5", label: "14:00-16:00" },
        { value: "6", label: "16:00-18:00" },
        { value: "7", label: "18:00-20:00" },
        { value: "8", label: "20:00-22:00" },
        { value: "9", label: "22:00-00:00" }
      ],
      staffoptions: [],
      hoteloptions: [
        { value: "0", label: "双人标间" },
        { value: "1", label: "商务大床" },
        { value: "2", label: "豪华大床" },
        { value: "3", label: "豪华套件" }
      ],
      departcitys: [],
      arrivecitys: [],
      restaurantsall: [],
      value8: "",
      form: {
        bookingtype: "2",
        traveltype: "1",
        departdate: "",
        arrivedate: "",
        departcity: "",
        arrivecity: "",
        travelway: "0",
        expectdeparttime: "",
        expectarrivetime: "",
        travelothers: "",

        hotelcheckindate: "",
        hotelcheckoutdate: "",
        hoteltype: "0",
        destination: "",
        hotellocation: "0",
        hotelothers: "",
        Passenger: [
          {
            PassengerName: "",
            PassengerCardNo:""
          }
        ],
        Apartment: [
          {
            ApartmentType: "0",
            Apartmentcount: 1
          }
        ]
      },
      airshow: true,
      hotelshow: true,
      destinationshow: false,
      restaurants: []
    };
  },
  methods: {
    onTravelTypeClick(type) {
      if (type == 1) {
        this.form.returndate = false;
      } else {
        this.form.returndate = true;
      }
    },
    onBookingTypeClick(type) {
      if (type == 0) {
        this.airshow = true;
        this.hotelshow = false;
      } else if (type == 1) {
        this.airshow = false;
        this.hotelshow = true;
      } else {
        this.airshow = true;
        this.hotelshow = true;
      }
    },
    addDomain() {
      if (this.form.Passenger.length < 9) {
        this.form.Passenger.push({
          value: "",
          key: Date.now()
        });
      } else {
        this.$message.error("添加出行人数不能超过9人！");
      }
    },
    removeDomain(item) {
      if (this.form.Passenger.length > 1) {
        var index = this.form.Passenger.indexOf(item);
        if (index !== -1) {
          this.form.Passenger.splice(index, 1);
        }
      } else {
        this.$message.error("最少出行人数不能超过1人！");
      }
    },
    addHDomain() {
      this.form.Apartment.push({
        value: "",
        key: Date.now()
      });
    },
    removeHDomain(item) {
      if (this.form.Apartment.length > 1) {
        var index = this.form.Apartment.indexOf(item);
        if (index !== -1) {
          this.form.Apartment.splice(index, 1);
        }
      } else {
        this.$message.error("最少房间数为1间！");
      }
    },
    changedepartdate() {
      this.form.hotelcheckindate = this.form.departdate;
    },
    changearrivedate() {
      this.form.hotelcheckoutdate = this.form.arrivedate;
    },
    onhotellocationClick(type) {
      if (type == 1) {
        this.destinationshow = true;
      } else {
        this.destinationshow = false;
      }
    },
    onSubmit(formName) {

      debugger;
      if (this.form.Passenger.length < this.form.Apartment.length) {
        this.$message.error("出行人数必须大于等于房间数！");
        return false;
      }

      if (this.form.bookingtype == 0) {
        this.form.hotelcheckindate = null;
        this.form.hotelcheckoutdate = null;
        this.form.hoteltype = null;
        this.form.destination = null;
        this.form.hotellocation = null;
        this.form.hotelothers = null;
      }

      this.$refs[formName].validate(valid => {
        if (valid) {
          debugger;
          this.$http
            .post(
              "/api/Enterprise/GenerateOrder",
              Service.Encrypt.DataEncryption({
                BookingType: this.form.bookingtype,
                TravelType: this.form.traveltype,
                DepartDate: this.form.departdate,
                ArriveDate: this.form.arrivedate,
                DepartCity: this.form.departcity,
                ArriveCity: this.form.arrivecity,
                TravelWay: this.form.travelway,
                ExpectDepartTime: this.form.expectdeparttime,
                ExpectArrivetime: this.form.expectarrivetime,
                TravelOthers: this.form.travelothers,

                HotelCheckinDate: this.form.hotelcheckindate,
                HotelCheckoutDate: this.form.hotelcheckoutdate,
                HotelType: this.form.hoteltype,
                Destination: this.form.destination,
                HotelLocation: this.form.hotellocation,
                HotelOthers: this.form.hotelothers,

                Passengers: this.form.Passenger,
                Apartments: this.form.Apartment,

                EnterpriseId: this.user.EnterpriseId,
                StaffId: this.user.StaffId
              })
            )
            .then(
              response => {
                if (
                  response.data.Data &&
                  response.data.Data != null &&
                  response.data.Data != undefined
                ) {
                  debugger;
                  if (response.data.Status == 100) {
                    this.$message.success("提交成功");
                    this.$router.push("waitconfirmtishi");
                  } else {
                    this.$message.error("提交失败");
                  }
                } else {
                  this.$message.error("提交失败");
                }
              },
              error => {
                this.$message("请求失败！");
                console.log(error);
              }
            );
        } else {
          this.$message.error("输入有误，请检查输入！");
          return false;
        }
      });
    },
    querydSearch(query) {
      if (query !== "") {
        this.departcitys = this.restaurantsall.filter(item => {
          return (
            item.City_PinYin.toLowerCase().indexOf(query.toLowerCase()) > -1 ||
            item.City_Name.toLowerCase().indexOf(query.toLowerCase()) > -1
          );
        });
      } else {
        this.departcitys = this.restaurantsall;
      }
    },
    queryaSearch(query) {
      if (query !== "") {
        this.arrivecity = this.restaurantsall.filter(item => {
          return (
            item.City_PinYin.toLowerCase().indexOf(query.toLowerCase()) > -1 ||
            item.City_Name.toLowerCase().indexOf(query.toLowerCase()) > -1
          );
        });
      } else {
        this.arrivecity = this.restaurantsall;
      }
    },
    loadAll() {
      this.$http
        .post("/api/Boss/GetT_AirPortsList", Service.Encrypt.DataEncryption({}))
        .then(
          response => {
            if (
              response.data.Data &&
              response.data.Data != null &&
              response.data.Data != undefined
            ) {
              if (response.data.Status == 100) {
                this.restaurantsall = response.data.Data;
                this.departcitys = response.data.Data;
                this.arrivecitys = response.data.Data;
              } else {
                this.restaurantsall = [];
              }
            } else {
              this.restaurantsall = [];
            }
          },
          error => {
            this.$message.error("请求失败！");
            console.log(error);
          }
        );
    },
    loadstafflist() {
      this.$http
        .post(
          "/api/Enterprise/GetStaffListChangeValue",
          Service.Encrypt.DataEncryption({
            EnterpriseId: this.user.EnterpriseId
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
                this.restaurants = response.data.Data;
              } else {
                this.staffoptions = [];
              }
            } else {
              this.staffoptions = [];
            }
          },
          error => {
            this.$message.error("请求失败！");
            console.log(error);
          }
        );
    },
    querySearch(queryString, cb) {
      
      var restaurants = this.restaurants;
      var results = queryString
        ? restaurants.filter(this.createFilter(queryString))
        : restaurants;
      // 调用 callback 返回建议列表的数据
      cb(results);
    },
    createFilter(queryString) {
      return restaurant => {
        return (
          restaurant.value.toLowerCase().indexOf(queryString.toLowerCase()) ===
          0
        );
      };
    },
    handleSelect(item,domain) {
       debugger;
       domain.PassengerCardNo =item.cardno;
    }
  },
  mounted() {
    this.user = JSON.parse(localStorage.getItem("ms_username"));
    this.loadAll();
    this.loadstafflist();
  }
};
</script>
<style scoped lang="scss">
.my-autocomplete {
  li {
    line-height: normal;
    padding: 7px;

    .name {
      text-overflow: ellipsis;
      overflow: hidden;
    }
    .addr {
      font-size: 12px;
      color: #b4b4b4;
    }

    .highlighted .addr {
      color: #ddd;
    }
  }
}
</style>