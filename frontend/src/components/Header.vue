<template>
  <header class="header-bg">
    <div class="header-inner">
      <router-link to="/app/home" class="logo">MOODI:ARY</router-link>

      <nav class="nav-links">
        <div class="menu-wrapper">
          <span class="menu-button">My Diary</span>
          <div class="submenu">
            <router-link to="/app/registmydiary">일기 작성</router-link>
            <router-link to="/app/monthlydiary">월간 일기</router-link>
            <router-link to="/app/weeklydiary">주간 일기</router-link>
          </div>
        </div>
        <div class="menu-wrapper">
          <span class="menu-button">Shared Diary</span>
          <div class="submenu">
            <router-link to="/app/shareddiary/room">우리 일기</router-link>
          </div>
        </div>
        <div class="menu-wrapper">
          <span class="menu-button">My Emotion</span>
          <div class="submenu">
            <router-link to="/app">감정 분석</router-link>
          </div>
        </div>
      </nav>

      <div class="right-controls">
        <!-- 로그인 후: 프로필 + 드롭다운 -->
        <template v-if="authStore.isLogin">
          <div class="profile-menu-wrapper" ref="profileMenu">
            <img
              src="@/assets/stickers/rabbit.png"
              alt="profile"
              class="profile-icon"
              @click.stop="toggleProfile"
            />
            <div class="submenu user">
              <router-link to="/app/mypage/info">내 정보</router-link>
              <router-link to="/app/mypage/pet">펫 관리</router-link>
              <router-link to="/app/mypage/recommend">추천 설정</router-link>
              <router-link @click="logout" to="/app">로그아웃</router-link>
            </div>
          </div>
        </template>

        <!-- 로그인 전: 로그인 버튼 -->
        <template v-else>
          <button class="btn login-btn" @click="goLogin">로그인</button>
        </template>
      </div>
    </div>
  </header>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'
import { useAuthStore } from '@/stores/auth'

const authStore = useAuthStore()
const showProfile = ref(false)

function toggleProfile() {
  showProfile.value = !showProfile.value
}

const profileMenu = ref(null)
function handleClickOutside(e) {
  if (profileMenu.value && !profileMenu.value.contains(e.target)) {
    showProfile.value = false
  }
}
onMounted(() => document.addEventListener('click', handleClickOutside))
onBeforeUnmount(() => document.removeEventListener('click', handleClickOutside))

const router = useRouter()
function goLogin()  { router.push('/login') }
function logout() {
  authStore.clear()
  localStorage.removeItem('token')
  delete axios.defaults.headers.common['Authorization']
  router.push('/app/home')
}
</script>

<style scoped>
.header-bg {
  position: fixed;
  top: 0; left: 0;
  width: 100vw; height: 80px;
  background-color: var(--color-brown);
  z-index: 9999;
}
.header-inner {
  max-width: 1920px;
  margin: 0 auto;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 40px;
  box-sizing: border-box;
}

.logo {
  color: var(--color-beige);
  font-family: var(--font-pixel);
  font-size: 2rem; font-weight: 800;
  text-decoration: none;
  text-shadow: -1px -1px 0 #A17C59;
}

.nav-links {
  display: flex; gap: 50px; align-items: center;
}
.menu-wrapper { position: relative; }
.menu-button {
  color: var(--color-beige);
  font-size: 24px; font-family: var(--font-omyu);
  cursor: pointer;
}
.submenu {
  position: absolute; top: 36px; left: 0;
  background-color: var(--color-beige);
  border: 2px solid var(--color-brown);
  border-radius: 20px; padding: 12px 20px;
  display: none; flex-direction: column; gap: 10px;
  min-width: 180px; box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

.user {
  position: absolute; top: 36px; left: -120px;
}

.menu-wrapper:hover .submenu {
  display: flex; animation: fadeIn 0.3s ease-in-out;
}
.submenu a {
  color: var(--color-brown);
  font-family: var(--font-omyu);
  font-size: 18px; text-decoration: none;
  transition: color 0.2s;
}
.submenu a:hover { color: var(--color-green); }
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-4px); }
  to   { opacity: 1; transform: translateY(0); }
}

.right-controls { display: flex; align-items: center; }

.btn {
  all: unset; padding: 6px 14px; border-radius: 6px;
  font-size: 14px; font-weight: 500; cursor: pointer;
  text-align: center;
}
.login-btn {
  background-color: #A17C59;
  color: #fff;
}
.login-btn:hover {
  background-color: rgba(0,0,0,0.2);
}

.profile-menu-wrapper { position: relative; }

.profile-menu-wrapper:hover .submenu {
    display: flex; animation: fadeIn 0.3s ease-in-out;
}

.profile-icon {
  width: 36px; height: 36px; border-radius: 50%;
  object-fit: cover; cursor: pointer;
}
.profile-dropdown {
  position: fixed; right: 0; top: 44px;
  background: var(--color-beige); border-radius: 6px;
  list-style: none; padding: 0; margin: 0;
  box-shadow: 0 2px 10px rgba(0,0,0,0.2);
}
.profile-dropdown li {
  padding: 8px 16px; color: var(--color-brown);
  cursor: pointer;
}
.profile-dropdown li:hover {
  background: rgba(0,0,0,0.05);
}
</style>
