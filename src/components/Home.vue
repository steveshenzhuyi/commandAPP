<template>
  <div align="center">
    <img src="./pictrue/logo.png">
    <h2>第五届世界浙商大会</h2>
    <h2>医疗保障系统指挥端</h2><hr>
    <mt-field label="用户名" placeholder="请输入用户名" v-model="userId"></mt-field>
    <!-- <mt-field label="密码" placeholder="请输入密码" type="password" v-model="pwd"></mt-field> -->
    <hr>
    <br>
    <mt-button size="large" type="primary" @click="check()">登录</mt-button>
    <br>
    <!-- <mt-button size="large" type="danger" @click="">忘记密码</mt-button> -->
    <router-view></router-view>
  </div>
</template>

<script>
import axios from 'axios';
import { Toast } from 'mint-ui';
import { MessageBox } from 'mint-ui';

export default {
  data() {
    return {
      userId: '',
      pwd: '123',
    };
  },
  mounted(){
    if(window.localStorage.getItem('VIDEOUSERID')!=null && window.localStorage.getItem('VIDEOUSERID')!=undefined){
      this.userId = window.localStorage.getItem('VIDEOUSERID')
    }
  },
  methods: {
    check() {
     if(this.userId == 901 || this.userId == 902 || this.userId == 903 || this.userId == 904 || this.userId == 905 || this.userId == 130){
      var videoid =  Number(this.userId)
      window.localStorage.setItem("VIDEOUSERID",videoid)
      this.$router.push({name:'command',params:{SELECTED:"指挥",SELECTED1:'1'}});
     }else{
      Toast('用户名或密码错误');
     }
    },
    
  }
};
</script>

