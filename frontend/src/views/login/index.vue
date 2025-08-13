<template>
  <div class="login-container">
    <div class="login-left">
      <img :src="login_image" alt="" />
    </div>
    <div class="login-content">
      <div class="login-right">
        <img width="300" height="72" :src="smartbiLogo" alt="SmartBI" />
        <div class="welcome">{{ $t('common.intelligent_questioning_platform') }}</div>
        <div class="login-form">
          <h2 class="title">{{ $t('common.login') }}</h2>
          <el-form
            ref="loginFormRef"
            class="form-content_error"
            :model="loginForm"
            :rules="rules"
            @keyup.enter="submitForm"
          >
            <el-form-item prop="username">
              <el-input
                v-model="loginForm.username"
                clearable
                :placeholder="$t('common.your_account_email_address')"
                size="large"
              ></el-input>
            </el-form-item>
            <el-form-item prop="password">
              <el-input
                v-model="loginForm.password"
                :placeholder="$t('common.enter_your_password')"
                type="password"
                show-password
                clearable
                size="large"
              ></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" class="login-btn" @click="submitForm">{{
                $t('common.login_')
              }}</el-button>
            </el-form-item>
          </el-form>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useUserStore } from '@/stores/user'
import { useI18n } from 'vue-i18n'
import smartbiLogo from '@/assets/embedded/smartbi-logo.svg'
import login_image from '@/assets/embedded/login_image.png'

const router = useRouter()
const userStore = useUserStore()
const { t } = useI18n()

const loginForm = ref({
  username: '',
  password: '',
})

const rules = {
  username: [{ required: true, message: t('common.your_account_email_address'), trigger: 'blur' }],
  password: [{ required: true, message: t('common.the_correct_password'), trigger: 'blur' }],
}

const loginFormRef = ref()

const submitForm = () => {
  loginFormRef.value.validate((valid: boolean) => {
    if (valid) {
      userStore.login(loginForm.value).then(() => {
        router.push('/chat')
      })
    }
  })
}
</script>

<style lang="less" scoped>
.login-container {
  height: 100vh;
  width: 100vw;
  background-color: var(--bg-color);
  display: flex;
  align-items: center;
  justify-content: center;

  .login-left {
    display: flex;
    height: 100%;
    img {
      height: 100%;
    }
  }

  .login-content {
    display: flex;
    align-items: center;
    justify-content: center;
    flex: 1;

    .login-right {
      display: flex;
      align-items: center;
      flex-direction: column;
      position: relative;
      .welcome {
        margin: 8px 0 40px 0;
        font-weight: 400;
        font-size: 14px;
        line-height: 20px;
        color: var(--text-light);
      }

      .login-form {
        border: 1px solid var(--border-color);
        padding: 40px;
        width: 480px;
        min-height: 392px;
        border-radius: 12px;
        box-shadow: 0px 6px 24px 0px #1f232914;
        background: var(--panel-bg);

        .form-content_error {
          .ed-form-item--default {
            margin-bottom: 24px;
            &.is-error {
              margin-bottom: 48px;
            }
          }
        }

        .title {
          font-weight: 500;
          font-style: Medium;
          font-size: 20px;
          line-height: 28px;
          margin-bottom: 24px;
        }

        .login-btn {
          width: 100%;
          height: 45px;
          font-size: 16px;
          border-radius: 4px;
        }

        .agreement {
          margin-top: 20px;
          text-align: center;
          color: #666;
        }
      }
    }
  }
}

:deep(.ed-input__wrapper) {
  background-color: #f5f7fa;
}
</style>
