<template>
  <!-- <div>创建新的分类</div> -->
  <el-form ref="form" :model="form" label-width="80px" style="height:70vh">
    <el-form-item label="分类名称">
      <el-input v-model="form.name"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="onSubmit" :plain="true">提交</el-button>
      <el-button>取消</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
import { adminRequest } from "network/request.js";
export default {
  props:["category","method"],
  name: "PostInfo",
  data() {
    return {
      form: {
        name: ""
      }
    };
  },
  watch:{
    category(value){
      this.form.name = this.category.name;
    }
  },
  methods: {
    onSubmit() {
      let sentData = {
        name:this.form.name,
        // id:this.category.id
      }
      if(this.method==="post"){
        sentData.id=this.category.id
      }
      adminRequest({
        url: "/category",
        method: this.method,
        data: sentData
      })
        .then(res => {
          if (res["res"] === 1) {
            this.$message({
              message: res["message"],
              type: "success"
            });
          } else {
            this.$message.error(res["message"]);
          }
        })
        .catch(err => {
          this.$message.error(res["message"]);
        });
    }
  }
};
</script>

<style scoped>
.el-form {
  display: flex;
  align-items: center;
}
</style>
