<script>
import EditModal from "@/components/EditModal.vue";
import users from "@/assets/data.JSON";
import { Message } from "element-ui";

export default {
  components: {
    EditModal,
  },
  data() {
    return {
      names: users.users,
      genders: users.genders,
      hardcoded: 10,
      search: "",
      searchGender: "",
    };
  },
  computed: {
    filteredNames() {
      return this.names.filter(
        (entry) =>
          entry.name.includes(this.search) &&
          (!this.searchGender || entry.gender == this.searchGender)
      );
    },
  },
  methods: {
    deleteRow(id) {
      this.names = this.names.filter((entry) => entry.id !== id);
    },
    createUser(user) {
      const date = new Date();
      const year = date.getFullYear();
      const month = date.getMonth() + 1;
      const day = date.getDate();

      this.names.push({
        id: this.hardcoded,
        name: user.name,
        gender: user.gender,
        cell: user.cell,
        date: `${year}-${month}-${day}`,
      });
      this.hardcoded += 1;
      Message.success("用户创建成功");
    },
    updateUser(updatedUser) {
      const user = this.names.find((entry) => entry.id === updatedUser.id);
      if (user) {
        user.name = updatedUser.name;
        user.gender = updatedUser.gender;
        user.cell = updatedUser.cell;
        Message.success("用户信息更新成功");
      }
    },
    resetSearch() {
      this.search = "";
      this.searchGender = "";
    },
  },
};
</script>

<template>
  <div>
    <div>
      用户名
      <el-input v-model="search" placeholder="用户名搜索" clearable />
      性别

      <el-select v-model="searchGender" placeholder="性别搜索" clearable>
        <el-option
          v-for="item in genders"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        />
      </el-select>

      <el-button class="reset" @click="resetSearch" icon="el-icon-refresh"
        >重置</el-button
      >
    </div>

    <EditModal :add="true" @finalize="createUser"></EditModal>

    <el-table class="el-table" :data="filteredNames">
      <el-table-column label="编号" prop="id" />
      <el-table-column label="用户名" prop="name" />
      <el-table-column label="性别" prop="gender" />
      <el-table-column label="手机号码" prop="cell" />
      <el-table-column label="添加日期" prop="date" />
      <el-table-column label="操作">
        <template slot-scope="scope">
          <EditModal
            :add="false"
            :user="scope.row"
            @finalize="
              (updatedUser) => (
                (scope.row.name = updatedUser.name),
                (scope.row.gender = updatedUser.gender),
                (scope.row.cell = updatedUser.cell)
              )
            "
          ></EditModal>
          <el-button
            type="danger"
            @click="deleteRow(scope.row.id)"
            icon="el-icon-delete"
            size="mini"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>

    <p v-if="filteredNames.length">总计: {{ filteredNames.length }}</p>
    <p v-else-if="!filteredNames.length">未找到匹配</p>
  </div>
</template>

<style>
.el-input {
  width: fit-content;
}

.el-select {
  width: fit-content;
}

.el-table {
  margin: auto;
  background-color: #fff;
  font-size: medium;
}

button:disabled {
  background-color: #c0c0c0;
  color: white;
  cursor: auto;
  text-decoration: line-through;
}

button:disabled:hover {
  background-color: #c0c0c0;
  color: white;
}

button {
  margin-left: 5px;
  padding: 3px 10px;
  border: none;
  border-radius: 5px;
  background-color: #98ddff;
  cursor: pointer;
}

button.new {
  border-radius: 10px;
  padding: 5px 10px;
  margin-top: 5px;
  margin-bottom: 5px;
  background-color: #84ffa3;
}

button.new:hover {
  background-color: #009b27;
  color: white;
}

button.reset {
  margin-left: 10px;
  background-color: #7f7f7f;
  color: white;
}

button.reset:hover {
  background-color: #cecece;
  color: black;
}

button.del {
  background-color: #ffa3a3;
}

button.del:hover {
  background-color: #ff5353;
  color: white;
}

button:hover {
  background-color: #0093dc; /* Blue background on hover */
  color: #ffffff;
}

.el-input {
  margin-right: 10px;
}

.el-select {
  margin-right: 10px;
}

p {
  margin-block-start: 0;
  margin-block-end: 0;
}
</style>
