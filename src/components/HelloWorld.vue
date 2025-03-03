<template>
  <div class="scaled">
    <div>
      <div v-if="currentView === 'welcome'" class="login-container">
        <div class="content-wrapper">
          <div>
            <h1 class="welcome-title">Welcome</h1>
            <span class="subtitle manage-expenses">Manage your expenses</span>
            <p class="subtitle seamless-text">seamlessly & intuitively</p>
          </div>
          <div class="button-group">
            <el-button class="primary-btn">Get Started</el-button>
            <el-button class="outline-btn">Explore Features</el-button>
          </div>
          <p class="existing-account">
            Already have an account?
            <a @click.prevent="currentView = 'login'">Sign in</a>
          </p>
        </div>
      </div>
      <!-- 登录页 -->
      <div v-if="currentView === 'login'" class="login-container">
        <div class="back-button" @click="currentView = 'welcome'">
          <el-icon>
            <ArrowLeft />
          </el-icon>
        </div>
        <h1 class="signin-title">Sign in</h1>
        <div class="form-container">
          <h2 class="welcome-back">Welcome Back</h2>
          <p class="greeting">Hello there, sign in to continue!</p>
          <div class="form-group">
            <el-form>
              <el-form-item class="custom-form-item">
                <el-form-item class="custom-form-item">
                  <span class="input-label">Label</span>
                  <el-input placeholder="Placeholder" class="input-field" v-model="loginForm.username" />
                </el-form-item>
                <span class="input-label">Label</span>
                <el-input placeholder="Placeholder" class="input-field" v-model="loginForm.password" type="password" />
              </el-form-item>
              <p class="forgot-password">
                <a href="#">Forgot Password?</a>
              </p>
              <el-button class="login-btn">Text</el-button>
            </el-form>
          </div>
          <p class="signup-prompt">
            Don't have an account?
            <a @click.prevent="currentView = 'register'">Sign up</a>
          </p>
        </div>
      </div>
      <!-- 注册页 -->
      <div v-if="currentView === 'register'" class="login-container">
        <div class="back-button" @click="currentView = 'welcome'">
          <el-icon>
            <ArrowLeft />
          </el-icon>
        </div>
        <h1 class="signin-title">Create account</h1>
        <div class="form-container">
          <h2 class="welcome-back">Welcome </h2>
          <p class="greeting">Hello there, sign up to continue.</p>
          <div class="form-group">
            <el-form>
              <el-form-item class="custom-form-item">
                <el-scrollbar height="200px">
                  <el-form-item class="custom-form-item">
                    <span class="input-label">Label1</span>
                    <el-input placeholder="Placeholder" class="input-field" v-model="registerForm.field1" />
                    <span class="input-label">Label2</span>
                    <el-input placeholder="Placeholder" class="input-field" v-model="registerForm.field2" />
                    <span class="input-label">Label3</span>
                    <el-input placeholder="Placeholder" class="input-field" v-model="registerForm.field3" />
                    <span class="input-label">Label4</span>
                    <el-input placeholder="Placeholder" class="input-field" v-model="registerForm.field3" />
                    <span class="input-label">Label5</span>
                    <el-input placeholder="Placeholder" class="input-field" v-model="registerForm.field3" />
                    <span class="input-label">Label6</span>
                    <el-input placeholder="Placeholder" class="input-field" v-model="registerForm.field3" />
                    <span class="input-label">Label7</span>
                    <el-input placeholder="Placeholder" class="input-field" v-model="registerForm.field3" />
                    <span class="input-label">Label8</span>
                    <el-input placeholder="Placeholder" class="input-field" v-model="registerForm.field3" />
                  </el-form-item>
                </el-scrollbar>
              </el-form-item>
              <div class="terms-agreement">
                <el-checkbox v-model="agreeTerms">
                  <span class="terms-text">By creating an account, you agree to our</span>
                  <br />
                  <a href="#" class="terms-link">Terms and Conditions</a>
                </el-checkbox>
              </div>
              <el-button class="login-btn">Text</el-button>
            </el-form>
          </div>
          <p class="signup-prompt">
            Already have an account?
            <a @click.prevent="currentView = 'login'">Sign in</a>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { ArrowLeft } from '@element-plus/icons-vue';

const currentView = ref('welcome');

const loginForm = ref({
username: '',
password: ''
});

const registerForm = ref({
field1: '',
field2: '',
field3: ''
});

const agreeTerms = ref(false);
</script>

<style>
html, body {
margin: 0;
padding: 0;
width: 100%;
height: 100%;
}

.app {
width: 100vw;
height: 100vh;
display: flex;
justify-content: center;
align-items: center;
overflow: hidden;
}

.scaled {
width: 375px;
height: 812px;
transform-origin: top left;
transform: scale( min( calc(100vw / 375), calc(100vh / 812) ) );
}

.content-wrapper {
position: absolute;
width: 305px;
height: 323px;
left: 35px;
top: 416px;
}

.welcome-title {
position: absolute;
width: 143px;
height: 38px;
left: 0;
top: 0;
color: rgb(255, 255, 255);
font-size: 32px;
font-weight: 700;
line-height: 38px;
margin: 0;
letter-spacing: -0.5px;
}

.manage-expenses {
position: absolute;
width: 193px;
height: 22px;
left: 0;
top: 68px;
color: rgb(255, 255, 255);
font-size: 18px;
font-weight: 400;
line-height: 22px;
white-space: nowrap;
}

.seamless-text {
position: absolute;
width: 252px;
height: 27px;
left: 0;
top: 96px;
color: rgb(255, 255, 255);
font-size: 22px;
font-weight: 500;
line-height: 26px;
letter-spacing: 0.2px;
}

.button-group {
position: absolute;
top: 141px;
left: 0;
width: 305px;
display: flex;
flex-direction: column;
gap: 16px;
box-sizing: border-box;
}

.primary-btn {
width: 305px !important;
height: 44px !important;
background: rgb(255, 255, 255) !important;
border-radius: 8px !important;
color: rgb(0, 65, 196) !important;
font-weight: 500 !important;
font-size: 16px !important;
padding: 0 !important;
border: none !important;
display: flex !important;
justify-content: center;
align-items: center;
margin: 0 !important;
}

.outline-btn {
width: 305px !important;
height: 44px !important;
background: transparent !important;
border: 2px solid rgb(255, 255, 255) !important;
border-radius: 8px !important;
color: rgb(255, 255, 255) !important;
font-weight: 500 !important;
font-size: 16px !important;
padding: 0 !important;
display: flex !important;
justify-content: center;
align-items: center;
margin: 0 !important;
box-sizing: border-box;
}

.el-button {
--el-button-padding-horizontal: 0 !important;
--el-button-padding-vertical: 0 !important;
}

.existing-account {
position: absolute;
width: 218px;
top: 306px;
left: 44px;
color: rgb(255, 255, 255);
font-size: 12px;
font-weight: 500;
line-height: 17px;
text-align: center;
}

.existing-account a {
color: inherit;
text-decoration: underline;
cursor: pointer;
margin-left: 4px;
}

.login-container {
position: absolute;
width: 375px;
height: 812px;
background: rgb(0, 65, 196);
overflow: hidden;
font-family: 'Rubik', sans-serif;
}

.back-button {
position: absolute;
left: 20px;
top: 60px;
color: white;
font-size: 24px;
cursor: pointer;
z-index: 100;
}

.signin-title {
position: absolute;
width: auto;
left: 35px;
top: 118px;
color: white;
font-size: 28px;
font-weight: 700;
line-height: 33px;
margin: 0;
z-index: 99;
}

.form-container {
position: absolute;
width: 375px;
height: 626px;
top: 186px;
background: rgb(255, 255, 255);
border-radius: 30px 30px 0 0;
padding: 35px 35px 0;
box-sizing: border-box;
}

.welcome-back {
position: relative;
width: auto;
color: rgb(0, 34, 81);
font-size: 24px;
font-weight: 700;
line-height: 28px;
margin: 0 0 8px;
}

.greeting {
color: rgb(122, 134, 154);
font-size: 14px;
font-weight: 400;
line-height: 20px;
margin-bottom: 40px;
}

.form-group {
position: relative;
}

.custom-form-item {
margin-bottom: 24px;
}

.custom-form-item ::v-deep .el-form-item__label {
color: rgb(0, 34, 81) !important;
font-family: 'Rubik', sans-serif;
font-size: 14px;
font-weight: 500;
margin-bottom: 8px;
}

.input-field {
width: 305px !important;
height: 44px !important;
background: rgb(242, 244, 247) !important;
border-radius: 8px !important;
box-shadow: inset 0px 1px 2px rgba(0, 0, 0, 0.04) !important;
}

.input-field ::v-deep .el-input__inner {
background: transparent !important;
border: none !important;
padding: 0 16px !important;
font-family: 'Rubik', sans-serif;
color: rgb(0, 34, 81) !important;
}

.forgot-password {
text-align: left;
margin: -12px 0 32px;
}

.forgot-password a {
color: rgb(0, 65, 196);
font-size: 13px;
font-weight: 500;
text-decoration: none;
}

.login-btn {
width: 305px !important;
height: 44px !important;
background: rgb(0, 65, 196) !important;
border: none !important;
border-radius: 8px !important;
color: white !important;
font-weight: 500 !important;
opacity: 1 !important;
}

.signup-prompt {
position: absolute;
bottom: 40px;
left: 50%;
transform: translateX(-50%);
text-align: center;
color: rgb(122, 134, 154);
font-size: 14px;
font-weight: 500;
width: 100%;
}

.signup-prompt a {
color: rgb(0, 65, 196);
text-decoration: none;
font-weight: 500;
}

.register-container {
position: absolute;
width: 375px;
height: 812px;
background: rgb(0, 65, 196);
overflow: hidden;
font-family: 'Rubik', sans-serif;
}

.register-form-container {
position: absolute;
width: 375px;
height: 689px;
top: 123px;
background: white;
border-radius: 30px 30px 0 0;
padding: 35px;
box-sizing: border-box;
}

.nav-bar {
display: flex;
align-items: center;
margin-bottom: 24px;
}

.back-button {
color: rgb(0, 34, 81);
font-size: 24px;
margin-right: 16px;
cursor: pointer;
}

.register-title {
color: rgb(0, 34, 81);
font-size: 28px;
font-weight: 700;
margin: 0;
}

.register-welcome {
color: rgb(0, 34, 81);
font-size: 24px;
font-weight: 700;
margin: 0 0 8px;
}

.register-subtitle {
color: rgb(122, 134, 154);
font-size: 14px;
margin-bottom: 40px;
}

.register-input-group {
position: relative;
margin-bottom: 32px;
}

.register-label {
position: absolute;
top: -20px;
left: 0;
color: rgb(193, 199, 208);
font-size: 14px;
font-weight: 500;
line-height: 16.59px;
}

.register-input {
width: 305px !important;
height: 44px !important;
background: rgb(242, 244, 247) !important;
border-radius: 8px !important;
box-shadow: inset 0px 1px 2px rgba(0, 0, 0, 0.04) !important;
}

.register-input ::v-deep .el-input__inner {
background: transparent !important;
border: none !important;
padding: 0 16px !important;
color: rgb(0, 34, 81) !important;
font-family: 'Rubik', sans-serif;
}

.terms-agreement {
margin: 24px 0 32px;
}

.terms-agreement ::v-deep .el-checkbox__label {
color: rgb(122, 134, 154);
font-size: 14px;
}

.terms-link {
color: rgb(0, 65, 196) !important;
text-decoration: none;
}

.register-submit-btn {
width: 305px !important;
height: 44px !important;
background: rgb(0, 65, 196) !important;
border: none !important;
border-radius: 8px !important;
color: white !important;
font-weight: 500 !important;
}

.register-existing-account {
position: absolute;
bottom: 40px;
left: 50%;
transform: translateX(-50%);
color: rgb(122, 134, 154);
font-size: 14px;
text-align: center;
width: 100%;
}

.register-existing-account a {
color: rgb(0, 65, 196);
text-decoration: none;
}

.register-container {
position: absolute;
width: 375px;
height: 812px;
background: rgb(0, 65, 196);
overflow: hidden;
font-family: 'Rubik', sans-serif;
}

.register-title {
position: absolute;
width: 219px;
height: 33px;
left: 35px;
top: 118px;
color: rgb(255, 255, 255);
font-size: 28px;
font-weight: 700;
line-height: 33.18px;
margin: 0;
z-index: 99;
}

.register-form-container {
position: absolute;
width: 375px;
height: 626px;
top: 186px;
background: white;
border-radius: 30px 30px 0 0;
padding: 35px;
box-sizing: border-box;
}

.back-button {
position: absolute;
left: 20px;
top: 60px;
color: white;
font-size: 24px;
cursor: pointer;
z-index: 100;
}

.register-welcome {
color: rgb(0, 34, 81);
font-size: 24px;
font-weight: 700;
margin: 0 0 8px;
}

.register-subtitle {
color: rgb(122, 134, 154);
font-size: 14px;
margin-bottom: 40px;
}

.register-input-group {
position: relative;
margin-bottom: 32px;
}

.register-label {
position: absolute;
top: -20px;
left: 0;
color: rgb(193, 199, 208);
font-size: 14px;
font-weight: 500;
line-height: 16.59px;
}

.register-input {
width: 305px !important;
height: 44px !important;
background: rgb(242, 244, 247) !important;
border-radius: 8px !important;
box-shadow: inset 0px 1px 2px rgba(0, 0, 0, 0.04) !important;
}

.register-input ::v-deep .el-input__inner {
background: transparent !important;
border: none !important;
padding: 0 16px !important;
color: rgb(0, 34, 81) !important;
font-family: 'Rubik', sans-serif;
}

.terms-agreement {
margin: 24px 0 32px;
}

.terms-agreement ::v-deep .el-checkbox__label {
color: rgb(122, 134, 154);
font-size: 14px;
}

.terms-link {
color: rgb(0, 65, 196) !important;
text-decoration: none;
}

.register-submit-btn {
width: 305px !important;
height: 44px !important;
background: rgb(0, 65, 196) !important;
border: none !important;
border-radius: 8px !important;
color: white !important;
font-weight: 500 !important;
}

/* 已有账户 */
.register-existing-account {
position: absolute;
bottom: 40px;
left: 50%;
transform: translateX(-50%);
color: rgb(122, 134, 154);
font-size: 14px;
text-align: center;
width: 100%;
}

.register-existing-account a {
color: rgb(0, 65, 196);
text-decoration: none;
}

/* 新增：统一所有页面容器的尺寸和定位 */
</style>
