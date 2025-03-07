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
              <h3 class="title">{{ $t('login.title') }}</h3>
              <p class="sub-title">{{ $t('login.subTitle') }}</p>
              <el-form
                ref="formRef"
                :model="formData"
                :rules="rules"
                @keyup.enter="handleSubmit"
                class="login-form"
              >
                <el-form-item prop="username">
                  <el-input
                    :placeholder="$t('login.placeholder[0]')"
                    size="large"
                    v-model.trim="formData.username"
                  />
                </el-form-item>
                <el-form-item prop="password">
                  <el-input
                    :placeholder="$t('login.placeholder[1]')"
                    size="small"
                    v-model.trim="formData.password"
                    type="password"
                    autocomplete="off"
                  />
                </el-form-item>

                <div class="forget-password">
                  <el-checkbox v-model="formData.rememberPassword">
                    {{ $t('login.rememberPwd') }}
                  </el-checkbox>
                  <router-link to="/forget-password">{{ $t('login.forgetPwd') }}</router-link>
                </div>

                <div class="form-footer">
                  <el-button
                    class="login-btn"
                    size="small"
                    type="primary"
                    @click="handleSubmit"
                    :loading="loading"
                    v-ripple
                  >
                    {{ $t('login.btnText') }}
                  </el-button>
                </div>

                <div class="footer">
                  <p>
                    {{ $t('login.noAccount') }}
                    <router-link to="/register">{{ $t('login.register') }}</router-link>
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
  import { ElMessage, ElNotification } from 'element-plus'
  import { useUserStore } from '@/store/modules/user'
  import { HOME_PAGE } from '@/router'
  import { ApiStatus } from '@/utils/http/status'
  import { useI18n } from 'vue-i18n'
  import type { FormInstance, FormRules } from 'element-plus'
  import { UserService } from '@/api/usersApi'
  import { LanguageEnum } from '@/enums/appEnum'
  import ShowcaseSlider from '@/components/Pages/Login/ShowcaseSlider.vue'

  const { t, locale } = useI18n()
  const userStore = useUserStore()
  const router = useRouter()
  const formRef = ref<FormInstance>()
  const systemName = SystemInfo.name

  // 语言配置
  const languageOptions = [
    { value: LanguageEnum.ZH, label: '简体中文' },
    { value: LanguageEnum.EN, label: 'English' }
  ]

  // 切换语言
  const changeLanguage = (lang: LanguageEnum) => {
    if (locale.value === lang) return
    locale.value = lang
    userStore.setLanguage(lang)
  }

  const formData = reactive({
    username: SystemInfo.login.username,
    password: SystemInfo.login.password,
    rememberPassword: true
  })

  const rules = computed<FormRules>(() => ({
    username: [{ required: true, message: t('login.placeholder[0]'), trigger: 'blur' }],
    password: [{ required: true, message: t('login.placeholder[1]'), trigger: 'blur' }]
  }))

  const loading = ref(false)

  const handleSubmit = async () => {
    if (!formRef.value) return

    await formRef.value.validate(async (valid) => {
      if (valid) {
        loading.value = true

        try {
          const res = await UserService.login({
            body: JSON.stringify({
              username: formData.username,
              password: formData.password
            })
          })

          if (res.code === ApiStatus.success && res.data) {
            userStore.setToken(res.data.accessToken)
            const userRes = await UserService.getUserInfo()
            if (userRes.code === ApiStatus.success) {
              userStore.setUserInfo(userRes.data)
            }
            userStore.setLoginStatus(true)
            showLoginSuccessNotice()
            router.push(HOME_PAGE)
          } else {
            ElMessage.error(res.message)
          }
        } finally {
          loading.value = false
        }
      }
    })
  }

  const showLoginSuccessNotice = () => {
    setTimeout(() => {
      ElNotification({
        title: t('login.success.title'),
        type: 'success',
        showClose: false,
        duration: 2500,
        zIndex: 10000,
        message: `${t('login.success.message')}, ${SystemInfo.name}!`
      })
    }, 300)
  }
</script>

<style lang="scss" scoped>
  @use './index.scss';
</style>
