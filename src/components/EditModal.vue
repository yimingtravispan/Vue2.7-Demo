<template>
  <div>
    <el-button
      v-if="add"
      :class="{ add: add }"
      type="primary"
      icon="el-icon-plus"
      @click="open"
      >添加</el-button
    >
    <el-button
      v-if="!add"
      type="primary"
      icon="el-icon-edit"
      @click="open"
      size="mini"
      >编辑</el-button
    >

    <div v-if="display" class="modal-mask">
      <div class="modal-container">
        <div class="modal-header">
          <slot v-if="add">新建用户</slot>
          <slot v-if="!add">编辑用户</slot>
        </div>

        <div class="modal-body">
          <input v-model="tempName" />
          <p class="error" v-if="!tempName">姓名不可为空</p>
        </div>

        <el-radio-group v-model="tempGender">
          <el-radio label="男">男</el-radio>
          <el-radio label="女">女</el-radio>
        </el-radio-group>

        <div class="modal-body">
          <input v-model="tempCell" />
          <p class="error" v-if="!tempCell">联系电话不可为空</p>
        </div>

        <div class="modal-footer">
          <button class="modal-default-button reset" @click="close">
            取消
          </button>
          <button
            class="modal-default-button"
            :disabled="!tempName || !tempGender || !tempCell"
            @click="
              submit({ name: tempName, gender: tempGender, cell: tempCell })
            "
          >
            完成
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    add: Boolean,
    user: Object,
  },
  data() {
    return {
      display: false,
      tempName: this.add ? "" : this.user.name,
      tempGender: this.add ? "" : this.user.gender,
      tempCell: this.add ? "" : this.user.cell,
    };
  },
  methods: {
    close() {
      this.display = false;
    },
    submit(user) {
      this.display = false;
      this.$emit("finalize", user);
    },
    open() {
      if (this.add) {
        this.tempName = "";
        this.tempGender = "";
        this.tempCell = "";
      }
      this.display = true;
    },
  },
};
</script>

<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
}

.modal-container {
  width: 300px;
  margin: auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 20px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
}

.modal-body {
  margin: 10px 0;
}

.modal-default-button {
  float: right;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-header {
  margin: 0 auto;
  font-size: larger;
  font-style: bold;
  color: #00aaff;
}

.error {
  margin-left: 5px;
  margin-top: -1px;
  margin-bottom: -1px;
  font-style: italic;
  font-size: 12px;
  color: #ff7878;
}

button.add {
  margin-top: 5px;
  margin-bottom: 5px;
  color: black;
  border: none;
  background-color: #74fe96;
}

button.add:hover {
  background-color: #a6febc;
  color: white;
}
</style>
