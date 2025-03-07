<template>
  <div class="login">
    <div class="left-wrap">
      <showcase-slider />
    </div>
    <div class="right-wrap">
      <div class="top-right-wrap">
        <el-dropdown @command="changeLanguage" popper-class="langDropDownStyle">
          <div class="btn language-btn">
            <i class="iconfont-sys icon-language">&#xe611;</i>
          </div>
          <template #dropdown>
            <el-dropdown-menu>
              <div v-for="lang in languageOptions" :key="lang.value" class="lang-btn-item">
                <el-dropdown-item
                  :command="lang.value"
                  :class="{ 'is-selected': locale === lang.value }"
                >
                  <span class="menu-txt">{{ lang.label }}</span>
                  <i v-if="locale === lang.value" class="iconfont-sys icon-check">&#xe621;</i>
                </el-dropdown-item>
              </div>
            </el-dropdown-menu>
          </template>
        </el-dropdown>
      </div>
      <div class="login-container">
        <div class="login-box">
          <div class="header">
            <h1>{{ systemName }}</h1>
          </div>
          <div class="login-wrap">
            <div class="form">
              <h3 class="title">{{ $t('register.title') }}</h3>
              <p class="sub-title">{{ $t('register.subTitle') }}</p>
              <el-form
                ref="formRef"
                :model="formData"
                :rules="rules"
                @keyup.enter="register"
                class="login-form"
              >
                <el-form-item prop="username">
                  <el-input
                    v-model.trim="formData.username"
                    :placeholder="$t('register.placeholder[0]')"
                    size="large"
                  />
                </el-form-item>

                <el-form-item prop="password">
                  <el-input
                    v-model.trim="formData.password"
                    :placeholder="$t('register.placeholder[1]')"
                    size="large"
                    type="password"
                    autocomplete="off"
                  />
                </el-form-item>

                <el-form-item prop="confirmPassword">
                  <el-input
                    v-model.trim="formData.confirmPassword"
                    :placeholder="$t('register.placeholder[2]')"
                    size="large"
                    type="password"
                    autocomplete="off"
                  />
                </el-form-item>

                <el-form-item prop="agreement">
                  <el-checkbox v-model="formData.agreement">
                    {{ $t('register.agreeText') }}
                    <router-link
                      style="color: var(--main-color); text-decoration: none"
                      to="/privacy-policy"
                      >{{ $t('register.privacyPolicy') }}</router-link
                    >
                  </el-checkbox>
                </el-form-item>

                <div class="form-footer">
                  <el-button
                    class="login-btn"
                    size="large"
                    type="primary"
                    @click="register"
                    :loading="loading"
                    v-ripple
                  >
                    {{ $t('register.submitBtnText') }}
                  </el-button>
                </div>

                <div class="footer">
                  <p>
                    {{ $t('register.hasAccount') }}
                    <router-link to="/login">{{ $t('register.toLogin') }}</router-link>
                  </p>
                </div>
              </el-form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { SystemInfo } from '@/config/setting'
  import { ElMessage } from 'element-plus'
  import type { FormInstance, FormRules } from 'element-plus'
  import { useI18n } from 'vue-i18n'
  import { LanguageEnum } from '@/enums/appEnum'
  import ShowcaseSlider from '@/components/Pages/Login/ShowcaseSlider.vue'

  const { t, locale } = useI18n()
  const router = useRouter()
  const formRef = ref<FormInstance>()
  const systemName = SystemInfo.name
  const loading = ref(false)

  // 语言配置
  const languageOptions = [
    { value: LanguageEnum.ZH, label: '简体中文' },
    { value: LanguageEnum.EN, label: 'English' }
  ]

  // 切换语言
  const changeLanguage = (lang: LanguageEnum) => {
    if (locale.value === lang) return
    locale.value = lang
  }

  const formData = reactive({
    username: '',
    password: '',
    confirmPassword: '',
    agreement: false
  })

  const validatePass = (rule: any, value: string, callback: any) => {
    if (value === '') {
      callback(new Error(t('register.placeholder[1]')))
    } else {
      if (formData.confirmPassword !== '') {
        formRef.value?.validateField('confirmPassword')
      }
      callback()
    }
  }

  const validatePass2 = (rule: any, value: string, callback: any) => {
    if (value === '') {
      callback(new Error(t('register.rule[0]')))
    } else if (value !== formData.password) {
      callback(new Error(t('register.rule[1]')))
    } else {
      callback()
    }
  }

  const rules = reactive<FormRules>({
    username: [
      { required: true, message: t('register.placeholder[0]'), trigger: 'blur' },
      { min: 3, max: 20, message: t('register.rule[2]'), trigger: 'blur' }
    ],
    password: [
      { required: true, validator: validatePass, trigger: 'blur' },
      { min: 6, message: t('register.rule[3]'), trigger: 'blur' }
    ],
    confirmPassword: [{ required: true, validator: validatePass2, trigger: 'blur' }],
    agreement: [
      {
        validator: (rule: any, value: boolean, callback: any) => {
          if (!value) {
            callback(new Error(t('register.rule[4]')))
          } else {
            callback()
          }
        },
        trigger: 'change'
      }
    ]
  })

  const register = async () => {
    if (!formRef.value) return

    try {
      await formRef.value.validate()
      loading.value = true

      // 模拟注册请求
      setTimeout(() => {
        loading.value = false
        ElMessage.success('注册成功')
        toLogin()
      }, 1000)
    } catch (error) {
      console.log('验证失败', error)
    }
  }

  const toLogin = () => {
    setTimeout(() => {
      router.push('/login')
    }, 1000)
  }
</script>

<style lang="scss" scoped>
  @use '../login/index.scss' as login;
</style>
