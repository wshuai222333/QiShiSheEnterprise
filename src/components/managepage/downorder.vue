<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-calendar"></i> 表单管理</el-breadcrumb-item>
                <el-breadcrumb-item>表单</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="form-box">
                <el-form ref="form" :model="form" label-width="80px">
                <el-form-item >
                    <el-steps :active="0">
                    <el-step title="步骤 1" description="这是一段很长很长很长的描述性文字"></el-step>
                    <el-step title="步骤 2" description="这是一段很长很长很长的描述性文字"></el-step>
                     <el-step title="步骤 3" description="这段就没那么长了"></el-step>
                    </el-steps>
                </el-form-item>
                <el-form-item label="日期时间">
                        <el-col :span="11">
                            <el-date-picker type="date" placeholder="选择日期" v-model="form.date1" style="width: 100%;"></el-date-picker>
                        </el-col>
                        <el-col class="line" :span="2">-</el-col>
                         <el-col :span="11">
                            <el-date-picker type="date" placeholder="选择日期" v-model="form.date1" style="width: 100%;"></el-date-picker>
                        </el-col>
                </el-form-item>
                <el-form-item label="城市选择">
                        <el-col :span="11">
                             <el-cascader :options="options" v-model="form.options" style="width: 100%;"></el-cascader>
                        </el-col>
                        <el-col class="line" :span="2"><i class="el-icon-arrow-right" ></i></el-col>
                         <el-col :span="11">
                             <el-cascader :options="options" v-model="form.options" style="width: 100%;"></el-cascader>
                        </el-col>
                </el-form-item> 
                <el-form-item label="表单名称">
                        <el-input v-model="form.name"></el-input>
                </el-form-item>
             
                 <el-form-item>
                 <p style="border-bottom:solid 1px #ccc;"></p>
                </el-form-item>
             
                <el-form-item label="单选框">
                        <el-radio-group v-model="form.resource">
                            <el-radio label="步步高"></el-radio>
                            <el-radio label="小天才"></el-radio>
                       </el-radio-group>
                </el-form-item>
                <el-form-item label="多选框">
                        <el-checkbox-group v-model="form.type">
                            <el-checkbox label="步步高" name="type"></el-checkbox>
                            <el-checkbox label="小天才" name="type"></el-checkbox>
                            <el-checkbox label="imoo" name="type"></el-checkbox>
                        </el-checkbox-group>
                
                </el-form-item>
                <el-form-item label="表单名称">
                        <el-input v-model="form.name"></el-input>
                </el-form-item>

                <el-form-item>
                 <p style="border-bottom:solid 1px #ccc;"></p>
                </el-form-item>

                <el-form-item label="单选框">
                        <el-radio-group v-model="form.resource">
                            <el-radio label="步步高"></el-radio>
                            <el-radio label="小天才"></el-radio>
                            <el-radio label="imoo"></el-radio>
                        </el-radio-group>
                </el-form-item>
                 <el-form-item label="表单名称">
                        <el-input v-model="form.name"></el-input>
                </el-form-item>
                 <el-form-item>
                 <p style="border-bottom:solid 1px #ccc;"></p>
                </el-form-item>
                <el-form-item label="城市级联" >
                     <el-row>
                     <el-col :span="6" >
                      <el-cascader :options="options" v-model="form.options"></el-cascader>
                     </el-col>
                      <el-col :span="6" :offset="1">
                      <el-cascader :options="options" v-model="form.options"></el-cascader>
                     </el-col>
                      <el-col :span="2" :offset="1">
                      
                      <el-input v-model="form.name"></el-input> 
                    
                     </el-col>
                     <el-col :span="2">
                      间             
                     </el-col>
                      <el-col :span="6">
                     <el-button type="primary" icon="el-icon-plus" size="small">添加同行人</el-button>
                     </el-col>
                     </el-row>
              </el-form-item>

                <el-form-item>
                        <el-button type="primary" @click="onSubmit">表单提交</el-button>
                </el-form-item>
                </el-form>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
        name: 'baseform',
        data: function(){
            return {
                options:[
                    {
                        value: 'guangdong',
                        label: '广东省',
                        children: [
                            {
                                value: 'guangzhou',
                                label: '广州市',
                                children: [
                                    {
                                        value: 'tianhe',
                                        label: '天河区'
                                    },
                                    {
                                        value: 'haizhu',
                                        label: '海珠区'
                                    }
                                ]
                            },
                            {
                                value: 'dongguan',
                                label: '东莞市',
                                children: [
                                    {
                                        value: 'changan',
                                        label: '长安镇'
                                    },
                                    {
                                        value: 'humen',
                                        label: '虎门镇'
                                    }
                                ]
                            }
                        ]
                    },
                    {
                        value: 'hunan',
                        label: '湖南省',
                        children: [
                            {
                                value: 'changsha',
                                label: '长沙市',
                                children: [
                                    {
                                        value: 'yuelu',
                                        label: '岳麓区'
                                    }
                                ]
                            }
                        ]
                    }
                ],
                form: {
                    name: '',
                    region: '',
                    date1: '',
                    date2: '',
                    delivery: true,
                    type: ['步步高'],
                    resource: '小天才',
                    desc: '',
                    options: []
                }
            }
        },
        methods: {
            onSubmit() {
                this.$message.success('提交成功！');
            }
        }
    }
</script>