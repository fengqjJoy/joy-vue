<template>
  <el-container style="height: 100vh;">
    <el-aside :width="sideWidth + 'px'"
              style="background-color: rgb(238, 241, 246); height: 100%;box-shadow: 2px 0 6px rgba(0,21,41,.35)">
      <el-menu :default-openeds="['1', '3']" style="min-height: 100%;overflow-x: hidden"
               background-color="rgb(48,65,86)"
               text-color="#fff" active-text-color="#ffd04b" :coTlapse-transition="false" :collapse="isCollapse">
        <div style="height: 60px; line-height: 60px; text-align: center">
          <img src="../assets/logo.png" alt="" style="width: 20px;position: relative;top:5px;margin-right: 5px">
          <b style="color: white" v-show="logoTextShow">后台管理系统</b>
        </div>
        <el-submenu index="1">
          <template slot="title"><i class="el-icon-message"></i>
            <span slot="title">导航一</span>
          </template>
          <el-menu-item-group title="分组2">
            <el-menu-item index="1-3">选项3</el-menu-item>
          </el-menu-item-group>
          <el-submenu index="1-4">
            <template slot="title">选项4</template>
            <el-menu-item index="1-4-1">选项4-1</el-menu-item>
          </el-submenu>
        </el-submenu>
        <el-submenu index="2">
          <template slot="title"><i class="el-icon-menu"></i><span slot="title">导航二</span></template>
          <el-menu-item-group title="分组2">
            <el-menu-item index="2-3">选项3</el-menu-item>
          </el-menu-item-group>
          <el-submenu index="2-4">
            <template slot="title">选项4</template>
            <el-menu-item index="2-4-1">选项4-1</el-menu-item>
          </el-submenu>
        </el-submenu>
        <el-submenu index="3">
          <template slot="title"><i class="el-icon-setting"></i><span slot="title">导航三</span></template>
          <el-menu-item-group title="分组2">
            <el-menu-item index="3-3">选项3</el-menu-item>
          </el-menu-item-group>
          <el-submenu index="3-4">
            <template slot="title">选项4</template>
            <el-menu-item index="3-4-1">选项4-1</el-menu-item>
          </el-submenu>
        </el-submenu>
      </el-menu>
    </el-aside>

    <el-container>
      <el-header style="font-size: 12px; line-height: 60px; border-bottom: 1px solid ; display: flex">
        <div style="flex: 1; font-size: 20px">
          <span :class="collapseBtnclass" style="cursor: pointer" @click="collapse"></span>
        </div>
        <el-dropdown style="width: 100px;cursor: pointer;">
          <span>王小虎</span><i class="el-icon-arrow-down" style="margin-left: 5px"></i>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>个人信息</el-dropdown-item>
            <el-dropdown-item>修改密码</el-dropdown-item>
            <el-dropdown-item>退出</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-header>
      <el-main>
        <div style="margin-bottom: 30px">
          <el-breadcrumb separator-class="el-icon-arrow-right">
            <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item>用户管理</el-breadcrumb-item>
          </el-breadcrumb>
        </div>
        <div style="margin: 10px 0;">
          <label >姓名 </label><el-input v-model="search" placeholder="请输入姓名" style="width: 200px" suffix-icon="el-icon-user"></el-input>
          <el-input v-model="email" placeholder="请输入邮箱" style="width: 200px" class="ml-10"
                    suffix-icon="el-icon-message"></el-input>
          <el-button type="primary" class="ml-10" icon="el-icon-search">搜索</el-button>
        </div>

        <div style="margin: 10px 0;">
          <el-button type="primary" size="mini" icon="el-icon-plus" plain>新增</el-button>
          <el-button type="danger" size="mini"  icon="el-icon-remove-outline" plain>批量删除</el-button>
          <el-button type="primary" size="mini"  icon="el-icon-remove-outline" plain>批量删除</el-button>
          <el-button type="primary" size="mini"  icon="el-icon-remove-outline" plain>批量删除</el-button>
        </div>

        <el-table :data="tableData" border stripe header-cell-class-name="headBg">
          <el-table-column prop="date" label="日期" width="140" align="center">
          </el-table-column>
          <el-table-column prop="name" label="姓名" width="120" align="center">
          </el-table-column>
          <el-table-column prop="address" label="地址" width="320" align="center">
          </el-table-column>
          <el-table-column label="操作">
            <template>
              <el-button type="success" plain><i class="el-icon-edit" size="mini"  ></i>编辑</el-button>
              <el-button type="danger" plain><i class="el-icon-remove-outline" size="mini"  ></i>删除</el-button>
            </template>
          </el-table-column>
        </el-table>
        <div style="padding: 10px 0">
          <el-pagination
              :page-sizes="[5, 10, 15, 20]"
              :page-size="10"
              layout="total, sizes, prev, pager, next, jumper"
              :total="200">
          </el-pagination>
        </div>
      </el-main>

    </el-container>
  </el-container>
</template>

<script>

export default {
  name: "home",
  data() {
    const item = {
      date: '2016-05-02',
      name: '王小虎',
      address: '上海市普陀区金沙江路 1518 弄'
    };
    return {
      tableData: Array(10).fill(item),
      collapseBtnclass: 'el-icon-s-fold',
      isCollapse: false,
      sideWidth: 200,
      logoTextShow: true,
      search: "",
      email: ""
    }
  },
  methods: {
    collapse() {
      this.isCollapse = !this.isCollapse
      if (this.isCollapse) {
        this.sideWidth = 64
        this.collapseBtnclass = 'el-icon-s-unfold'
        this.logoTextShow = false
      } else {
        this.sideWidth = 200
        this.collapseBtnclass = 'el-icon-s-fold'
        this.logoTextShow = true
      }
    }
  }
}
</script>
<style>
.el-header {
  color: #333;
  line-height: 60px;
}

.el-aside {
  color: #333;
}

.headBg {
  background: #eee !important;
}
</style>
