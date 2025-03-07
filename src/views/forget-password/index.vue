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
              <h3 class="title">{{ $t('forgetPassword.title') }}</h3>
              <p class="sub-title">{{ $t('forgetPassword.subTitle') }}</p>
              <el-form
                ref="formRef"
                :model="formData"
                :rules="rules"
                @keyup.enter="handleSubmit"
                class="login-form"
              >
                <el-form-item prop="username">
                  <el-input
                    :placeholder="$t('forgetPassword.placeholder')"
                    size="large"
                    v-model.trim="formData.username"
                  />
                </el-form-item>

                <div class="form-footer">
                  <el-button
                    class="login-btn"
                    size="large"
                    type="primary"
                    @click="handleSubmit"
                    :loading="loading"
                    v-ripple
                  >
                    {{ $t('forgetPassword.submitBtnText') }}
                  </el-button>
                </div>

                <div class="footer">
                  <p>
                    {{ $t('forgetPassword.backToLogin') }}
                    <router-link to="/login">{{ $t('forgetPassword.backBtnText') }}</router-link>
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
  import { useI18n } from 'vue-i18n'
  import type { FormInstance, FormRules } from 'element-plus'
  import { LanguageEnum } from '@/enums/appEnum'
  import ShowcaseSlider from '@/components/Pages/Login/ShowcaseSlider.vue'

  const { t, locale } = useI18n()
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
  }

  const formData = reactive({
    username: ''
  })

  const rules = computed<FormRules>(() => ({
    username: [{ required: true, message: t('forgetPassword.placeholder'), trigger: 'blur' }]
  }))

  const loading = ref(false)

  const handleSubmit = async () => {
    if (!formRef.value) return

    await formRef.value.validate(async (valid) => {
      if (valid) {
        loading.value = true
        // Add your forgot password logic here
        setTimeout(() => {
          loading.value = false
          router.push('/login')
        }, 1000)
      }
    })
  }
</script>

<style lang="scss" scoped>
  @use '../login/index.scss' as login;
</style>
