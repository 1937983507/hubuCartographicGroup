<template>
  <div class="home-container" ref="homeContainer">
    <nav class="navbar">
      <div class="nav-content">
        <div class="logo">
          <img src="/img/hubu-logo.png" alt="湖北大学" />
        </div>
        <!-- 桌面端导航菜单 -->
        <ul class="nav-menu desktop-menu">
          <li v-for="item in menuItems" :key="item.id">
            <a href="#" @click.prevent="showPage(item.id)" :class="{ active: currentPage === item.id }">
              {{ item.name }}
            </a>
          </li>
        </ul>
        <div class="nav-right">
          <!-- 移动端汉堡菜单按钮 -->
          <el-button 
            class="mobile-menu-btn" 
            @click="drawerVisible = true"
            :icon="Menu"
            circle
          />
          <a href="https://github.com/1937983507/hubuCartographicGroup" target="_blank" class="github-link" title="GitHub">
            <svg class="github-icon" viewBox="0 0 24 24" fill="currentColor" width="24" height="24">
              <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
            </svg>
          </a>
        </div>
      </div>
    </nav>

    <!-- 移动端抽屉 -->
    <el-drawer
      v-model="drawerVisible"
      title="导航菜单"
      :with-header="true"
      direction="rtl"
      size="280px"
      class="mobile-drawer"
    >
      <template #header>
        <div class="drawer-header">
          <div class="drawer-profile">
            <img src="/img/chengxiaoqiang.png" alt="成晓强" />
            <div class="drawer-name"><b>成晓强</b></div>
          </div>
        </div>
      </template>
      
      <!-- 个人信息卡片 - 折叠式 -->
      <el-collapse v-model="activeCollapseName" class="drawer-info-collapse">
        <el-collapse-item name="info" title="基本信息">
          <div class="info-content">
            <MarkdownContent :file="'personal-info.md'" />
          </div>
        </el-collapse-item>
      </el-collapse>

      <!-- 导航菜单 -->
      <el-menu 
        :default-active="currentPage" 
        class="drawer-menu"
        @select="handleMenuSelect"
      >
        <el-menu-item 
          v-for="item in menuItems" 
          :key="item.id" 
          :index="item.id"
        >
          {{ item.name }}
        </el-menu-item>
      </el-menu>
    </el-drawer>

    <div class="container" ref="container">
      <!-- 左侧内容 - 桌面端显示 -->
      <div class="container-left desktop-sidebar">
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
import { ref, shallowRef, onMounted, onUnmounted } from 'vue'
import { ArrowUp, Menu } from '@element-plus/icons-vue'
import MarkdownContent from '../components/MarkdownContent.vue'
import HomePage from '../components/pages/HomePage.vue'
import ProjectsPage from '../components/pages/ProjectsPage.vue'
import TeamAchievementsPage from '../components/pages/TeamAchievementsPage.vue'
import PapersPage from '../components/pages/PapersPage.vue'
import SoftwritingPatentPage from '../components/pages/SoftwritingPatentPage.vue'
import HonourPage from '../components/pages/HonourPage.vue'
import CooperationTeamPage from '../components/pages/CooperationTeamPage.vue'
import StudentTeamPage from '../components/pages/StudentTeamPage.vue'
import { recordPageVisit } from '@/utils/statistics'

const currentPage = ref('home')
const homeContainer = ref(null)
const rightContainer = ref(null)
const drawerVisible = ref(false)
const activeCollapseName = ref([]) // 默认折叠，空数组表示不展开

const menuItems = [
  { id: 'home', name: '首页' },
  { id: 'projects', name: '科研项目' },
  { id: 'papers', name: '学术论文' },
  { id: 'softwriting&patent', name: '软著专利' },
  { id: 'honour', name: '获奖荣誉' },
  { id: 'teamAchievements', name: '团队成果' },
  { id: 'cooperationTeam', name: '合作团队' },
  { id: 'studentTeam', name: '学生团队' }
]

const pageComponents = {
  home: HomePage,
  projects: ProjectsPage,
  teamAchievements: TeamAchievementsPage,
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
  // 移动端选择菜单后关闭抽屉
  if (drawerVisible.value) {
    drawerVisible.value = false
  }
}

const handleMenuSelect = (index) => {
  showPage(index)
}

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}

// 页面加载时记录访问量
onMounted(async () => {
  try {
    await recordPageVisit()
  } catch (error) {
    console.warn('记录页面访问失败:', error)
  }
})
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
    gap: 16px;
    width: 100%;
    max-width: 1400px;
    padding: 0 5%;
    justify-content: space-between;
  }

  .nav-right {
    display: flex;
    align-items: center;
    gap: 8px; // 汉堡菜单按钮和 GitHub 图标之间的间距

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
    flex: 1;
    list-style: none;
    margin: 0;
    padding: 0;
    gap: 0;
    justify-content: center;
    flex-wrap: nowrap;
    overflow: hidden;

    li {
      padding: 0 14px;
      line-height: 70px;
      cursor: pointer;
      transition: background 0.3s;
      white-space: nowrap;

      &:hover {
        background: rgba(240, 240, 240, 0.662);
      }

      a {
        text-decoration: none;
        color: #000;
        font-size: 18px;
        display: block;
        transition: color 0.3s;
        white-space: nowrap;

        &.active {
          font-weight: 600;
        }
      }
    }
  }

  // 移动端汉堡菜单按钮
  .mobile-menu-btn {
    display: none;
  }
}

.container {
  display: flex;
  max-width: 1400px;
  margin: 85px auto 20px;
  margin-top: 110px; /* 为固定的header留出空间 */
  gap: 20px;
  padding: 0 5%;
  position: relative;
}

.container-left {
  width: 300px;
  flex-shrink: 0;
  position: fixed;
  left: max(5%, calc((100% - 1500px) / 2 + 5%));
  top: 110px; /* 调整位置以适应固定的header */
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
      width: 40%;
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
  margin-left: 270px; /* 为左侧固定栏留出空间 */
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

// 移动端抽屉样式
.mobile-drawer {
  :deep(.el-drawer__header) {
    margin-bottom: 0 !important;
    padding: 20px;
    border-bottom: 1px solid #eee;
  }

  :deep(.el-drawer__body) {
    padding-top: 0 !important;
    padding: 20px;
  }

  .drawer-header {
    .drawer-profile {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 10px 0;

      img {
        max-width: 100px;
        max-height: 120px;
        width: auto;
        height: auto;
        object-fit: contain;
        margin-bottom: 10px;
        border-radius: 8px;
      }

      .drawer-name {
        font-size: 18px;
        color: #333;
      }
    }
  }

  .drawer-info-collapse {
    margin: 20px 0;
    border: none;

    :deep(.el-collapse-item) {
      background: rgba(255, 255, 255, 0.733);
      border-radius: 10px;
      border: 1px solid #e4e7ed;
      overflow: hidden;

      .el-collapse-item__header {
        padding: 15px 20px;
        font-size: 15px;
        font-weight: 600;
        color: #333;
        background: rgba(242, 248, 255, 0.608);
      }

      .el-collapse-item__content {
        padding: 15px 20px;
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

  .drawer-menu {
    border: none;
    margin-top: 10px;

    :deep(.el-menu-item) {
      height: 50px;
      line-height: 50px;
      font-size: 16px;
      margin-bottom: 5px;
      border-radius: 8px;

      &.is-active {
        background-color: #ecf5ff;
        color: #409eff;
      }
    }
  }
}

// 移动端响应式样式
@media (max-width: 768px) {
  .home-container {
    background: linear-gradient(to bottom, rgb(47, 179, 233) 0%, rgb(255, 255, 255) 100%) !important;
    background-attachment: scroll; // 移动端禁用固定背景
  }

  .navbar {
    height: 60px;

    .nav-content {
      padding: 0 15px;
    }

    .logo {
      img {
        height: 40px;
      }
    }

    // 移动端显示汉堡菜单，隐藏桌面菜单
    .mobile-menu-btn {
      display: flex !important;
      align-items: center;
      justify-content: center;
      width: 36px !important;
      height: 36px !important;
      min-width: 36px !important;
      min-height: 36px !important;
      padding: 0 !important;
      background: rgba(255, 255, 255, 0.5);
      border: 1px solid rgba(0, 0, 0, 0.1);
      
      :deep(.el-icon) {
        font-size: 20px;
        color: #333;
      }
    }

    .desktop-menu {
      display: none !important;
    }

    .nav-right {
      .github-link {
        width: 36px;
        height: 36px;

        .github-icon {
          width: 20px;
          height: 20px;
        }
      }
    }
  }

  .container {
    flex-direction: column;
    margin-top: 70px;
    padding: 0 15px;
    gap: 0;
  }

  .container-left {
    display: none; // 移动端隐藏左侧栏
  }

  .desktop-sidebar {
    display: none !important;
  }

  .container-right {
    width: 100%;
    margin-left: 0;
    padding: 0;
  }

  .back-top {
    right: 20px;
    bottom: 20px;
    
    :deep(.el-button) {
      width: 44px;
      height: 44px;
      font-size: 18px;
    }
  }
}

// 平板端适配 (768px - 1024px)
@media (min-width: 769px) and (max-width: 1024px) {
  .navbar {
    .nav-content {
      padding: 0 3%;
    }

    .nav-menu {
      li {
        padding: 0 10px;

        a {
          font-size: 16px;
        }
      }
    }
  }

  .container {
    padding: 0 3%;
  }

  .container-left {
    width: 250px;
    left: max(3%, calc((100% - 1200px) / 2 + 3%));
  }

  .container-right {
    margin-left: 220px;
    padding: 0 20px;
  }
}
</style>

