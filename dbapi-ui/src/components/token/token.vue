<template>
  <div>
    <div class="gap">
      <router-link to="/token/add">
        <el-button type="primary" plain>{{ $t('m.create_token') }}</el-button>
      </router-link>
    </div>

    <el-table :data="tableData" border stripe max-height="700" class="gap">
      <el-table-column prop="token" label="token" width="330px">
        <template slot-scope="scope">
          <span>{{ scope.row.token }}</span>
          <el-tag v-if="scope.row.expire != null && scope.row.expire < new Date().getTime()" type="danger" effect="dark"
                  size="mini">{{ $t('m.expired') }}
          </el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="note" :label="$t('m.note')"></el-table-column>
      <el-table-column prop="expire" :label="$t('m.expire')">
        <template slot-scope="scope">
          <span v-if="scope.row.expire == null">{{ $t('m.forever') }}</span>
          <span v-else>{{ scope.row.expire | dateFormat }}</span>
        </template>
      </el-table-column>

      <el-table-column prop="createTime" :label="$t('m.create_time')">
        <template slot-scope="scope">
          <span>{{ scope.row.createTime | dateFormat }}</span>
        </template>
      </el-table-column>

      <el-table-column :label="$t('m.operation')" width="100px">
        <template slot-scope="scope">
          <el-button plain size="mini" type="warning" @click="handleAuth(scope.row.id)" circle>
            <i class="el-icon-lock"></i>
          </el-button>
          <el-button plain size="mini" type="danger" @click="handleDelete(scope.row.id)" circle>
            <i class="el-icon-delete"></i>
          </el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog :title="$t('m.grant')" :visible.sync="dialogVisible" @open="getAllGroups">
      <el-checkbox-group v-model="checkList">
        <el-checkbox :label="item.id" v-for="item in groups">{{ item.name }}</el-checkbox>
      </el-checkbox-group>

      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">{{ $t('m.cancel') }}</el-button>
        <el-button type="primary" @click="dialogVisible = false;auth()">{{ $t('m.save') }}</el-button>
      </span>
    </el-dialog>

    <el-alert type="warning" show-icon>
      <div class="tip">{{ $t('m.token_tip') }}</div>
      <div><br/>
        import requests <br/>
        headers = {"Authorization": "5ad0dcb4eb03d3b0b7e4b82ae0ba433f"} <br/>
        re = requests.post("http://127.0.0.1:8520/api/userById", {"idList": [1, 2]}, headers=headers) <br/>
        print(re.text) <br/>
      </div>

    </el-alert>
  </div>
</template>

<script>
export default {
  name: "token",
  data() {
    return {
      tableData: [],
      dialogVisible: false,
      groups: [],
      checkList: [],
      tokenId: null
    }
  },
  methods: {
    getAll() {
      this.axios.post("/token/getAll").then((response) => {
        this.tableData = response.data
      }).catch((error) => {
      })
    },
    getAllGroups() {
      this.axios.post("/group/getAll/").then((response) => {
        this.groups = response.data
      }).catch((error) => {
      })
    },
    auth() {
      console.log(this.checkList)
      this.axios.post("/token/auth/", {tokenId: this.tokenId, groupIds: this.checkList.join(",")}).then((response) => {
        this.$message.success("Authorization Success")
      }).catch((error) => {
        this.$message.error("Authorization Failed")
      })
    },
    handleDelete(id) {
      this.axios.post("/token/delete/" + id).then((response) => {
        this.$message.success("Delete Success")
        this.getAll()
      }).catch((error) => {
        this.$message.error("Delete Failed")
      })
    },
    handleAuth(id) {
      this.dialogVisible = true
      this.tokenId = id
      this.axios.post("/token/getAuthGroups/" + id).then((response) => {
        this.checkList = response.data
      }).catch((error) => {
      })
    }

  },
  created() {
    this.getAll()
  }
}
</script>

<style scoped>
.tip {
  white-space: pre;
}

</style>