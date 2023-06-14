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
          <label>姓名: &nbsp; </label>
          <el-input v-model="username" placeholder="请输入姓名" style="width: 200px" suffix-icon="el-icon-user"></el-input>
          <label class="ml-5">邮箱: </label>
          <el-input v-model="email" placeholder="请输入邮箱" style="width: 200px" class="ml-10"
                    suffix-icon="el-icon-message"></el-input>
          <label class="ml-5">地址: </label>
          <el-input v-model="address" placeholder="请输入地址" style="width: 200px" class="ml-10"
                    suffix-icon="el-icon-message"></el-input>
          <el-button type="primary" @click="load" class="ml-10" icon="el-icon-search">搜索</el-button>
          <el-button type="warning" @click="reset" class="ml-10">重置</el-button>
        </div>

        <div style="margin: 10px 0;">
          <el-button type="primary" size="mini" @click="handleadd" icon="el-icon-plus" plain>新增</el-button>
          <el-popconfirm
              confirm-button-text='确定'
              cancel-button-text='我再想想'
              icon="el-icon-danger"
              icon-color="red"
              title="您确定删除吗？"
              @confirm="delbatch"
          >
          <el-button type="danger"  class="ml-10" slot="reference" size="mini" icon="el-icon-remove-outline" plain>批量删除</el-button>
          </el-popconfirm>
          <el-button type="primary" size="mini" class="ml-10" icon="el-icon-remove-outline" plain>批量删除</el-button>
          <el-button type="primary" size="mini" icon="el-icon-remove-outline" plain>批量删除</el-button>
        </div>

        <el-table :data="tableData" border stripe header-cell-class-name="headBg"
                  @selection-change="handleSelectionChange">
          <el-table-column type="selection" width="55"></el-table-column>
          <el-table-column prop="username" label="用户名" width="140" align="center"></el-table-column>
          <el-table-column prop="nickname" label="昵称" width="120" align="center"></el-table-column>
          <el-table-column prop="phone" label="电话" width="320" align="center"></el-table-column>
          <el-table-column prop="address" label="地址" width="320" align="center"></el-table-column>
          <el-table-column prop="email" label="邮箱" width="320" align="center"></el-table-column>
          <el-table-column label="操作">
            <template v-slot="scope">
              <el-button type="success" @click="handleedit(scope.row)" plain><i class="el-icon-edit" size="mini"></i>编辑
              </el-button>
              <el-popconfirm
                  confirm-button-text='确定'
                  cancel-button-text='我再想想'
                  icon="el-icon-info"
                  icon-color="red"
                  title="您确定删除吗？"
                  @confirm="del(scope.row.id)"
              >
                <el-button type="danger" slot="reference" class="ml-5" plain><i class="el-icon-remove-outline"
                                                                                size="mini"></i>删除
                </el-button>
              </el-popconfirm>
            </template>
          </el-table-column>
        </el-table>
        <div style="padding: 10px 0">
          <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="pageNum"
              :page-sizes="[5, 10, 15, 20]"
              :page-size="size"
              layout="total, sizes, prev, pager, next, jumper"
              :total="total">
          </el-pagination>
        </div>

        <el-dialog title="用户信息" :visible.sync="dialogFormVisible" width="30%">
          <el-form label-width="70px" size="small">
            <el-form-item label="用户名">
              <el-input v-model="form.username" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="昵称">
              <el-input v-model="form.nickname" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="电话">
              <el-input v-model="form.phone" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="邮箱">
              <el-input v-model="form.email" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="地址">
              <el-input v-model="form.address" autocomplete="off"></el-input>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible = false">取 消</el-button>
            <el-button type="primary" @click="save">确 定</el-button>
          </div>
        </el-dialog>

      </el-main>

    </el-container>
  </el-container>
</template>

<script>


export default {
  name: "home",
  data() {
    return {
      tableData: [],
      collapseBtnclass: 'el-icon-s-fold',
      isCollapse: false,
      sideWidth: 200,
      logoTextShow: true,
      dialogFormVisible: false,
      multipleSelection: [],
      form: {},
      username: "",
      email: "",
      address: "",
      total: 0,
      size: 5,
      pageNum: 1
    }
  },
  created() {
    this.load();
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
    load() {
      this.request.get("/user/page", {
        params: {
          pageNum: this.pageNum,
          pageSize: this.size,
          username: this.username,
          email: this.email,
          address: this.address
        }
      }).then(res => {
        this.tableData = res.data.records;
        this.total = res.data.total;
        this.size = res.data.size;
      })

      // fetch("http://localhost:9090/user/page?pageNum=" + this.pageNum + "&pageSize=" + this.size+"&username="+this.username+"&email="+this.email).then(res => res.json()).then(res => {
      //   this.tableData = res.data.records;
      //   this.total = res.data.total;
      //   this.size = res.data.size;
      // })
    },
    handleSizeChange(pageSize) {
      this.size = pageSize;
      this.load()
    },
    handleCurrentChange(pageNum) {
      this.pageNum = pageNum;
      this.load()
    },
    handleadd() {
      this.dialogFormVisible = true;
      this.form = {}
    },
    handleedit(row) {
      this.form = row
      this.dialogFormVisible = true;
    },
    del(id) {
      this.request.delete("/user/" + id, this.form).then(res => {
        if (res.data) {
          this.$message.success("删除成功！");
          this.dialogFormVisible = false;
          this.load()
        } else {
          this.$message.error("删除失败！");
        }
      })
    },
    delbatch(){
      let ids=this.multipleSelection.map(v=>v.id);//存入id
      this.request.post("/user/del/batch", ids).then(res => {
        if (res.data) {
          this.$message.success("批量删除成功！");
          this.dialogFormVisible = false;
          this.load()
        } else {
          this.$message.error("批量删除失败！");
        }
      })
    },
    handleSelectionChange(val) {//多选
      this.multipleSelection = val;
    },
    save() {
      this.request.post("/user", this.form).then(res => {
        if (res.data) {
          this.$message.success("保存成功！");
          this.dialogFormVisible = false;
          this.load()
        } else {
          this.$message.error("保存失败！");
        }
      })
    },
    reset() {
      this.username = "";
      this.email = "";
      this.address = ""
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
