<!-- 查看角色 -->
<template lang="html">
  <div class="box">
    <div>
      <div class="search-box">
        <input v-model="searchKey" class="input search-input" type="text" placeholder="请输入你要查找的角色ID">
        <button class="button" type="button" name="button">查找角色</button>
      </div>
        <button @click="switchModal()" class="button add-role-button" type="button" name="button">添加角色</button>
    </div>
    <table class="table">
      <thead>
        <tr>
          <th>ID</th>
          <th>用户名</th>
          <th>别名</th>
          <th>创建时间</th>
          <th>更新时间</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item,index) in roleData">
          <td>{{ item.id }}</td>
          <td>{{ item.name }}</td>
          <td>{{ item.display_name }}</td>
          <td>{{ item.created_at }}</td>
          <td>{{ item.updated_at }}</td>
          <td><button @click="deleteRole(index)" class="button" type="button" name="button">删除角色</button></td>
        </tr>
      </tbody>
    </table>

    <add-role ref="addRole" v-bind:is-show-modal="isShowModal"></add-role>
  </div>
</template>

<script>
import AddRole from './AddRole'
export default {
  data() {
    return {
      token: '',
      roleData: null,
      isShowModal: false,
      searchKey: null,
    }
  },
  components: {
    AddRole,
  },
  methods: {
    switchModal: function () {
      const that = this;
      that.$refs.addRole.switchModal();
    },
    deleteRole: function () {
      const that = this;
      let prompt = confirm("确认删除改角色吗？");
      if (prompt) {

      }
    },
    getRole: function () {
      const that = this;
      axios({
        method: 'get',
        url: `${this.GLOBAL.localDomain}/api/v1/roles/`,
        headers: {
          'Accept': 'application/json',
          'Authorization': that.token
        }
      }).then(res => {
        that.roleData = res.data.data;
      }).catch(err => {
        console.log(err)
      })
    },
    // 查找用户
    searchRole: function () {
      const that = this;
      axios({
        method: 'get',
        url: `${this.GLOBAL.localDomain}/api/v1/roles/${that.searchKey}`,
        headers: {
          'Accept': 'application/json',
          'Authorization': that.token
        }
      }).then(res => {
        that.roleData = [];
        that.roleData.push(res.data.data);
      }).catch(err => {
        console.log(err)
      })
    },
    // 删除用户
    deleteRole: function (index) {
      const that = this;
      let id = that.roleData[index]['id'];
      let prompt = confirm("确认删除改用户吗？");
      if (prompt) {
        axios({
          method: 'put',
          url: `${this.GLOBAL.localDomain}/api/v1/roles/${id}`,
          headers: {
            'Accept': 'application/json',
            'Authorization': that.token
          }
        }).then(res => {
          that.roleData = res.data.data;
        }).catch(err => {
          console.log(err)
        })
      }
    },
  },
  created() {
    this.token = sessionStorage.getItem('token');
    this.getRole();
  },
  watch: {
  }
}
</script>

<style lang="scss">
table {
  margin: 35px auto 0 auto;
}
.search-input {
  width: 200px;
  display: inline-block;
  margin-right: 10px;
}
.search-box {
  padding-right: 20px;
  display: inline-block;
  border-right: 1px solid #dedede;
}
.add-role-button {
  margin-left: 20px;
}
.box-item {
  margin-bottom: 20px;
  input  {
    display: inline-block;
    width: 300px;
  }
  label {
    display: inline-block;
    width: 130px;
  }
}
</style>
