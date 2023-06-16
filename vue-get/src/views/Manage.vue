<template>
  <el-container style="height: 100vh;">
    <el-aside :width="sideWidth + 'px'"
              style="background-color: rgb(238, 241, 246); height: 100%;box-shadow: 2px 0 6px rgba(0,21,41,.35)">
      <Aside :logoTextShow="logoTextShow" :isCollapse="isCollapse" />
    </el-aside>

    <el-container>
      <el-header style=" border-bottom: 1px solid ; ">
        <Header :collapseBtnclass="collapseBtnclass" @asideCollapse="collapse" />
      </el-header>
      <el-main>
        <router-view/>
      </el-main>

    </el-container>
  </el-container>
</template>

<script>

import Aside from "@/components/Aside";
import Header from "@/components/Header";

export default {
  name: "manage",
  data() {
    return {
      isCollapse: false,
      logoTextShow: true,
      collapseBtnclass: 'el-icon-s-fold',
      sideWidth: 200
    }
  },

  components: {
    Aside, Header
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
    },

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


</style>
