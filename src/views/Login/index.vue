<template>
  <div id="login">
    <div class="login-wrap">
      <!-- 登录注册按钮开始 -->
      <ul class="menu-tab">
        <li v-for="(item,index) in menuTab"
            @click="toggleMenu(item)"
            :class="{'current':item.current}"
            :key="index">{{item.text}}</li>
      </ul>
      <!-- 表单开始 -->
      <el-form :model="ruleForm"
               status-icon
               :rules="rules"
               ref="ruleForm"
               class="login-form">
        <el-form-item prop="userName"
                      class="item-form">
          <label>邮箱</label>
          <el-input type="text"
                    v-model="ruleForm.userName"
                    autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item prop="checkPass"
                      class="item-form">
          <label>密码</label>
          <el-input type="password"
                    v-model="ruleForm.password"
                    autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item prop="code"
                      class="item-form">
          <label>验证码</label>
          <el-row :gutter="10">
            <el-col :span="15">
              <el-input v-model="ruleForm.code"></el-input>
            </el-col>
            <el-col :span="9">
              <el-button class="block"
                         type="primary">获取验证码</el-button>
            </el-col>
          </el-row>

        </el-form-item>
        <el-form-item>
          <el-button type="primary"
                     class="login-block block"
                     @click="submitForm('ruleForm')">提交</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import { stripscript, validatePass, validateEmail, validateVCode } from '@/utils/validate';
export default {
  data () {


    var checkAge = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('年龄不能为空'));
      }
      setTimeout(() => {
        if (!Number.isInteger(value)) {
          callback(new Error('请输入数字值'));
        } else {
          if (value < 18) {
            callback(new Error('必须年满18岁'));
          } else {
            callback();
          }
        }
      }, 1000);
    };
    var validateUserName = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入用户名'));
      } else {
        callback();
      }
    };
    var validatePassword = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码'));
      } else {
        callback();
      }
    };
    return {
      menuTab: [
        {
          text: '登录',
          current: true,
        },
        {
          text: '注册',
          current: false,
        }
      ],

      ruleForm: {
        userName: '',
        password: '',
        code: ''
      },
      rules: {
        userName: [
          { validator: validateUserName, trigger: 'blur' }
        ],
        password: [
          { validator: validatePassword, trigger: 'blur' }
        ],
        code: [
          { validator: checkAge, trigger: 'blur' }
        ]
      }
    }
  },
  created () {

  },
  mounted () {

  },
  methods: {
    toggleMenu (item) {
      this.menuTab.forEach(element => {
        element.current = false;
      });
      item.current = true
    }
  },
}
</script>

<style lang='scss' scoped>
#login {
  background: #344a5f;
  height: 100vh;
}
.login-wrap {
  width: 300px;
  margin: auto;
  .menu-tab {
    text-align: center;
    li {
      display: inline-block;
      width: 88px;
      line-height: 36px;
      font-size: 14px;
      color: #fff;
      border-radius: 2px;
      cursor: pointer;
    }
  }
  .current {
    background-color: rgba(0, 0, 0, 0.1);
  }
}
.login-form {
  margin-top: 29px;
  label {
    display: block;
    margin-bottom: 3px;
    font-size: 14px;
    color: #fff;
  }
  .itme-form {
    margin-bottom: 13px;
  }
  .block {
    width: 100%;
    display: block;
  }
  .login-block {
    margin-top: 19px;
  }
}
</style>