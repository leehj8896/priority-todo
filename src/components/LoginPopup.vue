<template>
  <div class="login-container">
    <div class="popup-background"/>
    <div class="login-popup">
      <div class="popup-row">
        <p class="input-title">이메일</p>
        <input
          class="input-login-info"
          type="text"
          v-model="loginInfo.email"
        >
      </div>
      <div class="popup-row">
        <p class="input-title">비밀번호</p>
        <input
          class="input-login-info"
          type="text"
          v-model="loginInfo.password"
        >
      </div>
      <div class="popup-row">
        <p class="login-description">신규 회원이면 가입됩니다.</p>
      </div>
      <div class="popup-btn-container">
        <button class="cancel" v-on:click="onClickCancel">취소</button>
        <button class="submit" v-on:click="onClickLogin">확인</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'LoginPopup',
  props: {
  },
  data() {
    return {
      loginInfo: {
        email: '',
        password: '',
      }
    }
  },
  computed: {
  },
  methods: {
    onClickCancel() {
      console.log('LoginPopup > onClickCancel')
    },
    async onClickLogin() {
      console.log('LoginPopup > onClickLogin')
      if (!this.loginInfo.email || !this.loginInfo.password) return
      const isMember = await this.checkIsMember()
      // 회원이면 로그인
      let loginResult = false
      if (isMember) {
        console.log('isMember')
        const authResult = await this.authLogin()
        if (authResult) {
          // 로그인 성공
          console.log('login success')
          loginResult = true
        } else {
          // 로그인 실패
          console.log('login fail')
        }
      }
      // 신규이면 가입
      else {
        console.log('isNotMember')
        await this.createAccount()
        loginResult = true
      }
      if (loginResult) this.$emit('success-login')
    },
    async checkIsMember() {
      console.log('LoginPopup > checkIsMember')
      const { data: [ user ] } = await axios.get('http://localhost:3000/users', {
        params: {
          email: this.loginInfo.email,
        }
      })
      console.log('get users by email', this.loginInfo.email, 'result', user)
      return !!user
    },
    async createAccount() {
      console.log('LoginPopup > createAccount')
      const { data: user } = await axios.post('http://localhost:3000/users', this.loginInfo)
      console.log('post users', this.loginInfo, 'result', user)
    },
    async authLogin() {
      console.log('LoginPopup > authLogin')
      const { data: [ user ] } = await axios.get('http://localhost:3000/users', {
        params: this.loginInfo,
      })
      return !!user
    }
  }
}
</script>

<style>
p {
  margin: 0;
}
.login-container {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}
.popup-background {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background-color: grey;
  opacity: 0.9;
}
.login-popup {
  width: 60%;
  height: 100px;
  border: 1px solid black;
  background-color: white;
  position: relative;
}
.popup-row {
  display: flex;
  flex-direction: row;
}
.input-title {
  font-size: 0.8rem;
  flex-basis: 30%;
}
.input-login-info {
  width: 80px;
  flex-basis: 70%;
}
.popup-btn-container {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: end;
  position: absolute;
  bottom: 0;
}
.login-description {
  font-size: 0.7rem;
  color: blue;
  width: 100%;
  text-align: right;
}
</style>
