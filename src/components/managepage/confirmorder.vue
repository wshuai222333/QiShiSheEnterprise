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
            <el-table :data="tableData3" style="width: 100%">
                <el-table-column prop="date" label="日期" width="150"></el-table-column>
                <el-table-column label="配送信息">
                    <el-table-column prop="name" label="姓名" width="120"></el-table-column>
                    <el-table-column label="地址">
                        <el-table-column prop="province" label="省份" width="120"></el-table-column>
                        <el-table-column prop="city" label="市区" width="120"></el-table-column>
                        <el-table-column prop="address" label="地址" width="300"></el-table-column>
                        <el-table-column prop="zip" label="邮编" width="120"></el-table-column>
                    </el-table-column>
                </el-table-column>
            </el-table>
            
            <el-table :data="tableData3" style="width: 100%">
                <el-table-column prop="date" label="日期" width="150"></el-table-column>
                <el-table-column label="配送信息">
                    <el-table-column prop="name" label="姓名" width="120"></el-table-column>
                    <el-table-column label="地址">
                        <el-table-column prop="province" label="省份" width="120"></el-table-column>
                        <el-table-column prop="city" label="市区" width="120"></el-table-column>
                        <el-table-column prop="address" label="地址" width="300"></el-table-column>
                        <el-table-column prop="zip" label="邮编" width="120"></el-table-column>
                    </el-table-column>
                </el-table-column>
            </el-table>
        </div>
        <div class="container">
            <el-table :data="tableData3" style="width: 100%">
                <el-table-column prop="date" label="日期" width="150"></el-table-column>
                <el-table-column label="配送信息">
                    <el-table-column prop="name" label="姓名" width="120"></el-table-column>
                    <el-table-column label="地址">
                        <el-table-column prop="province" label="省份" width="120"></el-table-column>
                        <el-table-column prop="city" label="市区" width="120"></el-table-column>
                        <el-table-column prop="address" label="地址" width="300"></el-table-column>
                        <el-table-column prop="zip" label="邮编" width="120"></el-table-column>
                    </el-table-column>
                </el-table-column>
            </el-table>
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
