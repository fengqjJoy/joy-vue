<template>
    <div style="font-size: 20px;line-height: 60px;display: flex">
        <div style="flex: 1; ">
            <span :class="collapseBtnclass" style="cursor: pointer; font-size: 20px;" @click="collapse"></span>
            <el-breadcrumb separator-class="el-icon-arrow-right" separator="/" style="display: inline-block;margin-left: 10px;font-size: 15px">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>用户管理</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <el-dropdown style="width: 100px;cursor: pointer;">
            <span>王小虎</span><i class="el-icon-arrow-down" style="margin-left: 5px"></i>
            <el-dropdown-menu slot="dropdown">
                <el-dropdown-item>个人信息</el-dropdown-item>
                <el-dropdown-item>修改密码</el-dropdown-item>
                <el-dropdown-item>退出</el-dropdown-item>
            </el-dropdown-menu>
        </el-dropdown>
    </div>

</template>

<script>
    export default {
        name: "Header",
        props: {
            collapseBtnclass: String,
            collapse: Boolean
        }
    }
</script>

<style scoped>

</style>
