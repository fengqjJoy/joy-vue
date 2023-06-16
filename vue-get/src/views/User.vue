<template>
  <div>
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
        <el-button type="danger" class="ml-10" slot="reference" size="mini" icon="el-icon-remove-outline" plain>
          批量删除
        </el-button>
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
  </div>
</template>

<script>
export default {
  name: "User",
  data(){
    return{
      headBg:"headBg",
      tableData: [],
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
  methods:{
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
    delbatch() {
      let ids = this.multipleSelection.map(v => v.id);//存入id
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
.headBg {
  background: #eee !important;
}
</style>
