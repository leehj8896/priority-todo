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
          type="password"
          v-model="loginInfo.password"
        >
      </div>
      <div class="popup-row">
        <p class="login-description" v-bind:style="descriptionStyle">
          {{ description }}
        </p>
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
import { collection, query, where, getDocs, setDoc, doc } from "firebase/firestore"

export default {
  name: 'LoginPopup',
  props: {
  },
  data() {
    return {
      loginInfo: {
        email: '',
        password: '',
      },
      description: '신규 회원이면 가입됩니다.',
      descriptionStyle: {},
    }
  },
  computed: {
    isValidInput() {
      return this.isValidEmail && this.isValidPassword
    },
    isValidEmail() {
      const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/
      return emailRegex.test(this.loginInfo.email)
    },
    isValidPassword() {
      const password = this.loginInfo.password
      if (password.length < 8) return false

      // 비밀번호는 영문 대문자, 영문 소문자, 숫자, 특수문자 중 적어도 3가지 종류의 문자를 포함해야 합니다.
      const regexList = [
          /[A-Z]/, // 영문 대문자
          /[a-z]/, // 영문 소문자
          /[0-9]/, // 숫자
          /[^A-Za-z0-9]/ // 특수문자
      ]
      /**
       * 정규식 목록을 반복하여 비밀번호가 모든 조건을 충족하는지 확인합니다.
       * 3가지 이상의 조건을 충족하지 않으면 유효하지 않은 비밀번호로 판단합니다.
       */
      return regexList.filter((regex) => regex.test(password)).length >= 3
    }
  },
  watch: {
    loginInfo: {
      deep: true,
      handler() {
        if (this.isValidInput) {
          this.description = '입력 가능합니다!'
          this.descriptionStyle = {
            color: 'blue'
          }
        } else {
          this.description = '이메일, 비밀번호를 확인해주세요.'
          this.descriptionStyle = {
            color: 'red'
          }
        }
      },
    },
  },
  methods: {
    onClickCancel() {
      console.log('LoginPopup > onClickCancel')
    },
    async onClickLogin() {
      console.log('LoginPopup > onClickLogin')
      if (!this.loginInfo.email || !this.loginInfo.password) return
      if (!this.isValidInput) return
      const isMember = await this.checkIsMember()
      // 회원이면 로그인
      let loginResult = false
      let userInfo = {}
      if (isMember) {
        console.log('isMember')
        userInfo = await this.authLogin()
        if (userInfo) {
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
      if (loginResult) this.$emit('success-login', this.loginInfo)
    },
    async checkIsMember() {
      console.log('LoginPopup > checkIsMember')
      const q = query(collection(this.$db, "users"), where("email", "==", this.loginInfo.email))
      const querySnapshot = await getDocs(q)
      return !querySnapshot.empty
    },
    async createAccount() {
      console.log('LoginPopup > createAccount')
      // const { data: user } = await axios.post('http://localhost:3000/users', this.loginInfo)
      const usersRef = collection(this.$db, 'users')
      await setDoc(doc(usersRef), this.loginInfo)
    },
    async authLogin() {
      console.log('LoginPopup > authLogin')
      const q = query(
        collection(this.$db, "users"),
        where("email", "==", this.loginInfo.email),
        where("password", "==", this.loginInfo.password),
      )
      const querySnapshot = await getDocs(q)
      return !querySnapshot.empty
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
