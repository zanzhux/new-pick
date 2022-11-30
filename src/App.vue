<template>
  <div id="app">
    <h1>管理录入</h1>
    <div class="head">
      <el-row :gutter="20">
        <el-col :span="4">
          <el-input
            v-model="userInfo.name"
            placeholder="请输入你的供应商名"
          ></el-input>
        </el-col>
        <el-col :span="4">
          <el-input
            v-model="userInfo.position"
            placeholder="请输入你的地址"
          ></el-input>
        </el-col>
        <el-col :span="4">
          <el-input
            v-model="userInfo.phone"
            placeholder="请输入你的电话"
          ></el-input>
        </el-col>
        <el-col :span="4">
          <el-input
            v-model="userInfo.number"
            placeholder="请输入数量"
          ></el-input>
        </el-col>
        <el-col :span="4">
          <el-input v-model="userInfo.major" placeholder="是否入库"></el-input>
        </el-col>
        <el-col :span="4">
          <el-input v-model="userInfo.PickNotAny" placeholder="备注"></el-input>
        </el-col>
      </el-row>
      <el-button type="primary" @click="addUser" class="add-btn" plain
        >添加信息</el-button
      >
    </div>
    <!-- 主体内容 -->
    <div class="body">
      <template>
        <el-table :data="tableData" style="width: 100%">
          <el-table-column prop="number" label="数量" width="180"></el-table-column>
          <el-table-column
            prop="position"
            label="地址"
            width="180"
          ></el-table-column>
          <el-table-column prop="name" label="联系人"></el-table-column>
          <el-table-column prop="phone" label="电话"></el-table-column>
          <el-table-column prop="number" label="状态"></el-table-column>
          <el-table-column prop="PickNotAny" label="备注"></el-table-column>
          <el-table-column prop="position" label="操作">
            <template slot-scope="scope">
              <el-button
                type="primary"
                icon="el-icon-edit"
                @click="editUser(scope.row, scope.$index)"
                circle
              ></el-button>
              <el-button
                type="danger"
                icon="el-icon-delete"
                @click="delUser(scope.$index)"
                circle
              ></el-button>
            </template>
          </el-table-column>
        </el-table>
      </template>
    </div>
    <!-- 编辑框 -->
    <el-dialog
      title="编辑用户信息"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="handleClose"
    >
      <div>
        <el-form ref="form" :model="editObj" label-width="80px">
          <el-form-item label="数量"
            ><el-input v-model="editObj.number"></el-input
          ></el-form-item>
          <el-form-item label="地址"
            ><el-input v-model="editObj.position"></el-input
          ></el-form-item>
          <el-form-item label="联系人"
            ><el-input v-model="editObj.name"></el-input
          ></el-form-item>
          <el-form-item label="电话"
            ><el-input v-model="editObj.phone"></el-input
          ></el-form-item>
          <el-form-item label="状态"
            ><el-input v-model="editObj.number"></el-input
          ></el-form-item>
          <el-form-item label="备注"
            ><el-input v-model="editObj.PickNotAny"></el-input
          ></el-form-item>
        </el-form>
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="confirm">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      userInfo: {
        name: "",
        position: "",
        major: "",
        number: "",
        phone: "",
        PickNotAny: "",
      },
      tableData: [
        {
          name: "互联网",
          position: "北京",
          phone: "123456788",
          PickNotAny: "",
          major: "对外贸易",
          number: "未入库",
        },
        {
          name: "互联网",
          position: "北京",
          phone: "123456788",
          PickNotAny: "",
          major: "对外贸易",
          number: "未入库",
        },
        {
          name: "互联网",
          position: "北京",
          phone: "123456788",
          PickNotAny: "",
          major: "对外贸易",
          number: "未入库",
        },
      ],
      dialogVisible: false,
      editObj: {
        name: "",
          position: "",
          phone: "",
          PickNotAny: "",
          major: "",
          number: "",
      },
      userIndex: 0,
    };
  },
  methods: {
    //添加
    addUser() {
      if (!this.userInfo.name) {
        this.$message({
          message: "请输入你的供应商名！",
        });
        return;
      }
      if (!this.userInfo.position) {
        this.$message({
          message: "请输入你的地址！",
          type: "warning",
        });
        return;
      }
      if (!this.userInfo.major) {
        this.$message({
          message: "请输入你的电话！",
          type: "warning",
        });
        return;
      }
      if (!this.userInfo.number) {
        this.$message({
          message: "请输入数量！",
          type: "warning",
        });
        return;
      }
      this.tableData.push(this.userInfo);
      this.userInfo = {
        name: "",
        position: "",
        major: "",
        number: "",
      };
    },

    //删除
    delUser(idx) {
      this.$confirm("确认删除此用户信息？")
        .then((_) => {
          this.tableData.splice(idx, 1);
        })
        .catch((_) => {});
    },
    //编辑
    editUser(item, idx) {
      this.userIndex = idx;
      this.editObj = {
        name: item.name,
        position: item.position,
        major: item.major,
        number: item.number,
      };
      this.dialogVisible = true;
    },

    handleClose() {
      this.dialogVisible = false;
    },

    confirm() {
      this.dialogVisible = false;
      Vue.set(this.tableData, this.userIndex, this.editObj);
    },
  },
};
</script>
<style>
#app {
  width: 1024px;
  margin: 0 auto;
}
.add-btn {
  margin-top: 20px;
  width: 100%;
}
.body {
  margin-top: 20px;
}
</style>

