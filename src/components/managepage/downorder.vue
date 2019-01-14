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
              <el-cascader :options="options" v-model="form.options" placeholder="06:00-9:00"></el-cascader>
            </el-form-item>
            <el-form-item label="其他要求">
              <el-input v-model="form.travelothers" placeholder="头等舱/公务舱/指定航班(选填)"></el-input>
            </el-form-item>
          </div>

          <div v-show="hotelshow">
            <el-form-item label="目的地">
              <el-input v-model="form.name" placeholder="具体商圈/街区(选填)"></el-input>
            </el-form-item>

            <el-form-item>
              <p style="border-bottom:solid 1px #ccc;"></p>
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
              <p style="border-bottom:solid 1px #ccc;"></p>
            </el-form-item>
            <el-form-item label="房间信息">
              <el-row>
                <el-col :span="6">
                  <el-cascader :options="options" v-model="form.options" placeholder="选择房型"></el-cascader>
                </el-col>
                <el-col :span="2" :offset="1">
                  <el-input :v-model="1">1</el-input>
                </el-col>
                <el-col :span="2">间</el-col>
                <el-col :span="6" :offset="1">
                  <el-button type="primary" icon="el-icon-plus" size="small" round>添加房型</el-button>
                </el-col>
              </el-row>
            </el-form-item>
          </div>
          <el-form-item label="出行人">
            <el-row>
              <el-col :span="6">
                <el-cascader :options="options" v-model="form.options" placeholder="选择员工"></el-cascader>
              </el-col>
              <el-col :span="6" :offset="1">
                <el-button type="primary" icon="el-icon-plus" size="small" round>添加出行人</el-button>
              </el-col>
            </el-row>
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
      form: {
        bookingtype: "2",
        traveltype: "1",
        departtime:"",
        arrivetime:"",
        departcity:"",
        arrivecity:"",
        travelway: "0",
        expecttraveltime:"",
        travelothers:"",

        destination:"",
        hotellocation: "1",
        hotelothers:"",

        hotelinfo:[],

        traveluser:[],

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
    }
  }
};
</script>