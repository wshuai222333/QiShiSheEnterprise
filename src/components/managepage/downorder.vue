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
      <div class="form-box">
        <el-form ref="form" :model="form" label-width="100px">
          <el-form-item>
            <p style="border-bottom:1px;"></p>
          </el-form-item>
          <el-form-item label="预定类型">
            <el-radio-group v-model="form.bookingtype">
              <el-radio-button label="0" @click.native="onBookingTypeClick(0)">机票</el-radio-button>
              <el-radio-button label="1" @click.native="onBookingTypeClick(1)">酒店</el-radio-button>
              <el-radio-button label="2" @click.native="onBookingTypeClick(2)">机票&&酒店</el-radio-button>
            </el-radio-group>
          </el-form-item>

          <div v-show="airshow">
            <el-form-item label="行程类型">
              <el-radio-group v-model="form.traveltype">
                <el-radio-button label="0" @click.native="onTravelTypeClick(0)">单程</el-radio-button>
                <el-radio-button label="1" @click.native="onTravelTypeClick(1)">往返</el-radio-button>
              </el-radio-group>
            </el-form-item>
            <el-form-item label="行程日期">
              <el-col :span="11">
                <el-date-picker
                  type="date"
                  placeholder="出发日期"
                  v-model="form.date1"
                  style="width: 100%;"
                ></el-date-picker>
              </el-col>
              <el-col class="line" :span="2">-</el-col>
              <el-col :span="11">
                <el-date-picker
                  :disabled="form.returndate"
                  type="date"
                  placeholder="返回日期"
                  v-model="form.date1"
                  style="width: 100%;"
                ></el-date-picker>
              </el-col>
            </el-form-item>
            <el-form-item label="行程城市">
              <el-col :span="11">
                <el-cascader
                  :options="options"
                  v-model="form.options"
                  style="width: 100%;"
                  placeholder="出发城市"
                ></el-cascader>
              </el-col>
              <el-col class="line" :span="2">
                <i class="el-icon-arrow-right"></i>
              </el-col>
              <el-col :span="11">
                <el-cascader
                  :options="options"
                  v-model="form.options"
                  style="width: 100%;"
                  placeholder="到达城市"
                ></el-cascader>
              </el-col>
            </el-form-item>
            <el-form-item label="期望交通方式">
              <el-radio-group v-model="form.travelway">
                <el-radio-button label="0">飞机</el-radio-button>
                <el-radio-button label="1">火车</el-radio-button>
              </el-radio-group>
            </el-form-item>
            <el-form-item label="期望出行时间">
              <el-select v-model="form.expecttraveltime" placeholder="期望时间段">
                <el-option
                  v-for="item in expecttraveltime"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="其他要求">
              <el-input v-model="form.travelothers" placeholder="头等舱/公务舱/指定航班(选填)"></el-input>
            </el-form-item>

            <el-form-item>
              <p style="border-bottom:solid 1px #ccc;"></p>
            </el-form-item>
            <p style="height:4px;"></p>
          </div>

          <div v-show="hotelshow">
            <el-form-item label="目的地">
              <el-input v-model="form.destination" placeholder="具体商圈/街区(选填)"></el-input>
            </el-form-item>
            <el-form-item label="期望酒店位置">
              <el-radio-group v-model="form.hotellocation">
                <el-radio-button label="1">目的地</el-radio-button>
                <el-radio-button label="2">机场/车站</el-radio-button>
                <el-radio-button label="0">无需预定</el-radio-button>
              </el-radio-group>
            </el-form-item>
            <el-form-item label="其他要求">
              <el-input v-model="form.hotelothers" placeholder="其他位置/具体房间要求(选填)"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" icon="el-icon-plus" size="small" round>添加房型</el-button>
            </el-form-item>
            <el-form-item label="房间信息">
              <el-row>
                <el-col :span="6">
                  <el-select v-model="form.hotelinfo[0].hoteloption" placeholder="选择房型">
                    <el-option
                      v-for="item in hoteloptions"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                    ></el-option>
                  </el-select>
                </el-col>
                <el-col :span="2" :offset="1">
                  <el-input v-model="form.hotelinfo[0].roomcount"></el-input>
                </el-col>
                <el-col :span="2">间</el-col>
              </el-row>
            </el-form-item>
            <el-form-item>
              <p style="border-bottom:solid 1px #ccc;"></p>
            </el-form-item>
          </div>
          <!-- <el-form-item label="出行人">
            <el-row>
              <el-col :span="6">
                <el-select v-model="form.traveluser[0].staffname" placeholder="选择出行人">
                  <el-option
                    v-for="item in staffoptions"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value"
                  ></el-option>
                </el-select>
              </el-col>
              <el-col :span="6" :offset="1">
                <el-button type="primary" icon="el-icon-plus" size="small" round>添加出行人</el-button>
              </el-col>
            </el-row>
          </el-form-item>-->
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
            v-for="(domain, index) in dynamicValidateForm.domains"
            :label="'域名' + index"
            :key="domain.key"
            :prop="'domains.' + index + '.value'"
            :rules="{
      required: true, message: '出行人不能为空', trigger: 'blur'
    }"
          >
            <el-select v-model="domain.value" placeholder="选择出行人">
              <el-option
                v-for="item in staffoptions"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>

            <el-button @click.prevent="removeDomain(domain)">删除</el-button>
          </el-form-item>

          <el-form-item>
            <el-button type="primary" @click="onSubmit">提交</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "baseform",
  data: function() {
    return {
      options: [
        {
          value: "guangdong",
          label: "广东省",
          children: [
            {
              value: "guangzhou",
              label: "广州市",
              children: [
                {
                  value: "tianhe",
                  label: "天河区"
                },
                {
                  value: "haizhu",
                  label: "海珠区"
                }
              ]
            },
            {
              value: "dongguan",
              label: "东莞市",
              children: [
                {
                  value: "changan",
                  label: "长安镇"
                },
                {
                  value: "humen",
                  label: "虎门镇"
                }
              ]
            }
          ]
        },
        {
          value: "hunan",
          label: "湖南省",
          children: [
            {
              value: "changsha",
              label: "长沙市",
              children: [
                {
                  value: "yuelu",
                  label: "岳麓区"
                }
              ]
            }
          ]
        }
      ],
      expecttraveltime: [
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
      hoteloptions: [
        { value: "0", label: "双人标间" },
        { value: "1", label: "商务大床" },
        { value: "2", label: "豪华套间" }
      ],
      staffoptions: [
        { value: "王帅", label: "王帅" },
        { value: "李欢", label: "李欢" },
        { value: "张翰", label: "张翰" }
      ],
      dynamicValidateForm: {
        domains: [
          {
            value: ""
          }
        ]
      },
      form: {
        bookingtype: "2",
        traveltype: "1",
        departtime: "",
        arrivetime: "",
        departcity: "",
        arrivecity: "",
        travelway: "0",
        expecttraveltime: "0",
        travelothers: "",

        destination: "",
        hotellocation: "1",
        hotelothers: "",

        hotelinfo: [{ hoteloption: "0", roomcount: 1 }],

        traveluser: [{ staffname: "" }],

        name: "",
        region: "",
        date1: "",
        date2: "",
        delivery: true,
        type: ["步步高"],

        desc: "",
        options: [],
        returndate: false
      },
      airshow: true,
      hotelshow: true
    };
  },
  methods: {
    onSubmit() {
      this.$message.success("提交成功！");
    },
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
      this.dynamicValidateForm.domains.push({
        value: "",
        key: Date.now()
      });
    },
    removeDomain(item) {
      var index = this.dynamicValidateForm.domains.indexOf(item);
      if (index !== -1) {
        this.dynamicValidateForm.domains.splice(index, 1);
      }
    }
  }
};
</script>