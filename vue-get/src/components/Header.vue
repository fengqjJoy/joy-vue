<template>
  <div style="font-size: 20px;line-height: 60px;display: flex">
    <div style="flex: 1; ">
      <span :class="collapseBtnclass" style="cursor: pointer; font-size: 20px;" @click="collapse"></span>
      <el-breadcrumb separator-class="el-icon-arrow-right" separator="/"
                     style="display: inline-block;margin-left: 10px;font-size: 15px">
        <el-breadcrumb-item :to="'/'"  >首页</el-breadcrumb-item>
        <el-breadcrumb-item>{{ currentPathName }}</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <el-dropdown style="width: 70px;cursor: pointer;">
      <span>王小虎</span><i class="el-icon-arrow-down" style="margin-left: 5px"></i>
      <el-dropdown-menu slot="dropdown" style="width: 100px;text-align: center">
        <el-dropdown-item style="font-size: 14px; padding: 5px 0">个人信息</el-dropdown-item>
        <el-dropdown-item style="font-size: 14px; padding: 5px 0">修改密码</el-dropdown-item>
        <el-dropdown-item style="font-size: 14px; padding: 5px 0">退出</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
  </div>

</template>

<script>
export default {
  name: "Header",
  props: {
    collapseBtnclass: String
  },
  computed: {
    currentPathName () {
      return this.$store.state.currentPathName;　　//需要监听的数据
    }
  },
  watch: {
    currentPathName (newVal, oldVal) {
      console.log(newVal)
    }
  },
  methods: {
    collapse() {
      // this.$parent.$parent.$parent.$parent.collapse()  // 通过4个 $parent 找到父组件，从而调用其折叠方法
      this.$emit("asideCollapse")
    }
  }
}

</script>

<style scoped>

</style>
