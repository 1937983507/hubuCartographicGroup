<template>
  <div class="home-container" ref="homeContainer">
    <nav class="navbar">
      <div class="nav-content">
        <div class="logo">
          <img src="/img/hubu-logo.png" alt="湖北大学" />
        </div>
        <ul class="nav-menu">
          <li v-for="item in menuItems" :key="item.id">
            <a href="#" @click.prevent="showPage(item.id)" :class="{ active: currentPage === item.id }">
              {{ item.name }}
            </a>
          </li>
        </ul>
        <div class="nav-right">
          <a href="https://github.com/1937983507/hubuCartographicGroup" target="_blank" class="github-link" title="GitHub">
            <svg class="github-icon" viewBox="0 0 24 24" fill="currentColor" width="24" height="24">
              <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
            </svg>
          </a>
        </div>
      </div>
    </nav>

    <div class="container" ref="container">
      <!-- 左侧内容 -->
      <div class="container-left">
        <div class="profile-card">
          <img src="/img/chengxiaoqiang.png" alt="成晓强" />
          <div class="name"><b>成晓强</b></div>
        </div>
        <div class="info-card">
          <div class="info-title">
            <div>基本信息</div>
            <div>Personal Information</div>
          </div>
          <div class="info-content">
            <MarkdownContent :file="'personal-info.md'" />
          </div>
        </div>
      </div>

      <!-- 右侧内容 -->
      <div class="container-right" ref="rightContainer">
        <component :is="currentComponent" :show-page="showPage" />
        
        <!-- 返回顶端 -->
        <div class="back-top">
          <el-button circle @click="scrollToTop" :icon="ArrowUp" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, shallowRef } from 'vue'
import { ArrowUp } from '@element-plus/icons-vue'
import MarkdownContent from '../components/MarkdownContent.vue'
import HomePage from '../components/pages/HomePage.vue'
import ProjectsPage from '../components/pages/ProjectsPage.vue'
import PapersPage from '../components/pages/PapersPage.vue'
import SoftwritingPatentPage from '../components/pages/SoftwritingPatentPage.vue'
import HonourPage from '../components/pages/HonourPage.vue'
import CooperationTeamPage from '../components/pages/CooperationTeamPage.vue'
import StudentTeamPage from '../components/pages/StudentTeamPage.vue'

const currentPage = ref('home')
const homeContainer = ref(null)
const rightContainer = ref(null)

const menuItems = [
  { id: 'home', name: '首页' },
  { id: 'projects', name: '科研项目' },
  { id: 'papers', name: '学术论文' },
  { id: 'softwriting&patent', name: '软著专利' },
  { id: 'honour', name: '获奖荣誉' },
  { id: 'cooperationTeam', name: '合作团队' },
  { id: 'studentTeam', name: '学生团队' }
]

const pageComponents = {
  home: HomePage,
  projects: ProjectsPage,
  papers: PapersPage,
  'softwriting&patent': SoftwritingPatentPage,
  honour: HonourPage,
  cooperationTeam: CooperationTeamPage,
  studentTeam: StudentTeamPage
}

const currentComponent = shallowRef(HomePage)

const showPage = (pageId) => {
  currentPage.value = pageId
  currentComponent.value = pageComponents[pageId] || HomePage
  scrollToTop()
}

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}
</script>

<style scoped lang="scss">
.home-container {
  min-height: 100vh;
  background: url('/img/hubu2.png') top center no-repeat;
  background-size: cover;
  background-attachment: fixed;
  overflow-y: auto;
  position: relative;
}

.navbar {
  background-color: rgba(255, 255, 255, 0.3);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  height: 70px;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);

  .nav-content {
    display: flex;
    align-items: center;
    gap: 20px;
    width: 100%;
    max-width: 1400px;
    padding: 0 5%;
    justify-content: space-between;
  }

  .nav-right {
    display: flex;
    align-items: center;

    .github-link {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      transition: all 0.3s;
      text-decoration: none;
      color: #000;

      .github-icon {
        width: 24px;
        height: 24px;
        transition: all 0.3s;
        color: #000;
      }

      &:hover {
        background: rgba(240, 240, 240, 0.662);
        transform: scale(1.1);

        .github-icon {
          color: #409eff;
        }
      }
    }
  }

  .logo {
    display: flex;
    align-items: center;
    height: 100%;

    img {
      height: 50px;
    }
  }

  .nav-menu {
    display: flex;
    list-style: none;
    margin: 0;
    padding: 0;
    gap: 0;

    li {
      padding: 0 20px;
      line-height: 70px;
      cursor: pointer;
      transition: background 0.3s;

      &:hover {
        background: rgba(240, 240, 240, 0.662);
      }

      a {
        text-decoration: none;
        color: #000;
        font-size: 20px;
        display: block;
        transition: color 0.3s;

        &.active {
          font-weight: 600;
        }
      }
    }
  }
}

.container {
  display: flex;
  max-width: 1400px;
  margin: 85px auto 20px;
  margin-top: 155px; /* 为固定的header留出空间 */
  gap: 20px;
  padding: 0 5%;
  position: relative;
}

.container-left {
  width: 300px;
  flex-shrink: 0;
  position: fixed;
  left: max(5%, calc((100% - 1400px) / 2 + 5%));
  top: 155px; /* 调整位置以适应固定的header */
  height: fit-content;
  max-height: calc(100vh - 175px);
  overflow-y: auto;

  .profile-card {
    background: rgba(255, 255, 255, 0.733);
    border-radius: 15px;
    padding: 15px 0 10px;
    text-align: center;
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;

    img {
      width: 50%;
      margin-bottom: 10px;
    }

    .name {
      font-size: 16px;
      color: #333;
      text-align: center;
    }
  }

  .info-card {
    background: rgba(255, 255, 255, 0.733);
    border-radius: 15px;
    padding: 30px;

    .info-title {
      border-bottom: 1px solid rgb(194, 194, 194);
      padding-bottom: 10px;
      margin-bottom: 15px;
      font-size: 17px;
      font-weight: 600;

      div:first-child {
        font-size: 17px;
        font-weight: 600;
        color: #333;
        margin-bottom: 5px;
      }

      div:last-child {
        font-size: 13px;
        color: #666;
      }
    }

    .info-content {
      font-size: 13px;
      color: #666;
      line-height: 20px;

      p {
        margin-bottom: 8px;
      }
    }
  }
}

.container-right {
  flex: 1;
  background: transparent;
  padding: 0px 30px;
  position: relative;
  margin-left: 320px; /* 为左侧固定栏留出空间 */
}

.back-top {
  position: fixed;
  right: 5%;
  bottom: 5%;
  z-index: 999;
  
  :deep(.el-button) {
    width: 50px;
    height: 50px;
    background: #d2d2d2;
    border: none;
    color: #fff;
    font-size: 20px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
    transition: all 0.3s;
    
    &:hover {
      background: #565656;
      transform: translateY(-2px);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
    }
  }
}
</style>

