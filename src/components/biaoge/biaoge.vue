<template>
    <el-card class="card">
        <el-breadcrumb separator-class="el-icon-arrow-right">
            <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item>活动管理</el-breadcrumb-item>
            <el-breadcrumb-item>活动列表</el-breadcrumb-item>
            <el-breadcrumb-item>活动详情</el-breadcrumb-item>
        </el-breadcrumb>
        <el-row class="mt">
            <el-col :span="24">
                <el-input placeholder="请输入内容" class="input-with-select kuan">
                <!-- <el-select v-model="select" slot="prepend" placeholder="请选择">
                </el-select> -->
                    <el-button slot="append" icon="el-icon-search"></el-button>
                </el-input>
                <el-button type="success" plain>成功按钮</el-button>  
            </el-col>
        </el-row>
        <el-table
            border
            stripe
            :data="data"
            style="width: 100%">
            <el-table-column
                type="index"
                width="50">
            </el-table-column>
            <el-table-column
                prop="username"
                label="姓名"
                width="180">
            </el-table-column>
            <el-table-column
                prop="email"
                label="邮箱"
                width="180">
            </el-table-column>
            <el-table-column
                prop="mobile"
                label="电话"
                width="180">
            </el-table-column>
            <el-table-column
                label="时间"
                width="180">
                <template slot-scope="scope">
                    {{scope.row.create_time | fmtData('YYYY-MM-DD') }}
                </template>
            </el-table-column>
            <el-table-column
                label="用户状态"
                width="80">
                <template slot-scope="scope">
                    <!-- {{ scope.row.mg_state }} -->
                    <el-switch
                        v-model="scope.row.mg_state"
                        active-color="#13ce66"
                        inactive-color="#ff4949">
                    </el-switch>
                </template>
            </el-table-column>
            <el-table-column
                prop="mg_state"
                label="操作">
                <template slot-scope="scope">
                    <el-row>
                        <el-button 
                            type="primary" 
                            icon="el-icon-edit"
                            size="mini"
                            plain></el-button>
                        <el-button
                            type="danger"
                            icon="el-icon-delete"
                            size="mini"
                            plain></el-button>
                        <el-button
                            type="success"
                            icon="el-icon-check"
                            size="mini"
                            plain></el-button>
                    </el-row>
                </template>
            </el-table-column>
            </el-table>
    </el-card>
    
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {
            data: []
        };
    },
    created () {
        this.loadData();
    },
    methods: {
        async loadData() {
            var token = sessionStorage.getItem('token');
            axios.defaults.headers.common['Authorization'] = token;
            var response = await axios
            .get('http://localhost:8888/api/private/v1/users?pagenum=1&pagesize=10');
            var {meta: {status, msg} } = response.data;
            // console.log(response);
            if(status === 200) {
                this.data = response.data.data.users;
                console.log(this.data);
            } else {
                this.$message.error(msg);
            }
        }
    }
    
}
</script>

<style>

.kuan {
    width: 300px;
}

.mt {
    margin-top: 20px;
}
</style>
