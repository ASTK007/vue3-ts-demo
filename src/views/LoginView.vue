<template>
  <div class="login-box">
    <el-form
      ref="ruleFormRef"
      :model="ruleForm"
      status-icon
      :rules="rules"
      label-width="80px"
      class="demo-ruleForm"
    >
      <h2>后台管理系统</h2>
      <el-form-item label="账号：" prop="username">
        <el-input v-model="ruleForm.name" autocomplete="off" />
      </el-form-item>

      <el-form-item label="密码：" prop="password">
        <el-input
          v-model="ruleForm.password"
          type="password"
          autocomplete="off"
        />
      </el-form-item>
      <el-form-item>
        <el-button
          class="loginBtn"
          type="primary"
          @click="submitForm(ruleFormRef)"
          >登录</el-button
        >
        <el-button class="loginBtn" @click="resetForm">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, ref } from "vue";
import { LoginData } from "../type/login";
import type { FormInstance } from "element-plus";
import { login } from "../request/api";
import { useRouter } from "vue-router";
export default defineComponent({
  setup() {
    const data = reactive(new LoginData());
    const rules = {
      name: [
        {
          required: true,
          message: "请输入你的账号",
          trigger: "blur",
        },
        {
          max: 25,
          message: "账号的长度小于25",
          trigger: "blur",
        },
      ],
      password: [
        {
          required: true,
          message: "请输入你的密码",
          trigger: "blur",
        },
        {
          min: 3,
          max: 10,
          message: "密码的长度在3到10之间",
          trigger: "blur",
        },
      ],
    };
    // 登录
    const ruleFormRef = ref<FormInstance>();
    const router=useRouter()//-->$router
    const submitForm = (formEl: FormInstance | undefined) => {
      if (!formEl) return;
      // 对表单的内容进行验证
      // valid布尔类型，为true表示验证成功，反之
      formEl.validate((valid) => {
        if (valid) {
          // console.log("submit!");
          login(data.ruleForm).then((res) => {
            // console.log(res);
            // 将token进行保存
            localStorage.setItem("token", res.data.token);
            // 跳转页面，首页
            if(res.data.code==1){
              router.push('/')
            }else{
              alert("用户名或密码错误")
            }
          });
        } else {
          console.log("error submit!");
          return false;
        }
      });
      // console.log(formEl);
    };
    // 重置
    const resetForm = () => {
      data.ruleForm.name = "";
      data.ruleForm.password = "";
    };
    return { ...toRefs(data), rules, resetForm, ruleFormRef, submitForm };
  },
});
</script>

<style lang='scss' scoped>
.login-box {
  width: 100%;
  height: 100%;
  background: url("../assets/bg.jpg");
  padding: 1px;
  text-align: center;
  .demo-ruleForm {
    width: 500px;
    margin: 200px auto;
    background-color: #fff;
    padding: 40px;
    border-radius: 20px;
  }
  .loginBtn {
    width: 48%;
  }
  h2 {
    margin-bottom: 10px;
  }
}
</style>