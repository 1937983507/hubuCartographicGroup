<template>
  <div class="home-page">
    <!-- 实验室简介 -->
    <div class="page-section">
      <div class="section-title">
        <el-icon class="section-icon"><OfficeBuilding /></el-icon>
        <span class="section-title-left">实验室简介</span>
      </div>
      <div class="section-content">
        <MarkdownContent file="lab-introduction.md" />
      </div>
    </div>

    <!-- 团队成果 -->
    <div class="page-section">
      <div class="section-title">
        <el-icon class="section-icon"><Trophy /></el-icon>
        <span class="section-title-left">团队成果</span>
        <span class="section-title-right" @click="goToTeamAchievements">
          more >
        </span>
      </div>
      <div class="section-content achievements-carousel-wrapper">
        <el-carousel :height="isMobile ? undefined : '450px'" :trigger="carouselTrigger" indicator-position="outside" v-if="teamAchievements.length" class="team-achievements-carousel">
          <el-carousel-item v-for="(item, idx) in teamAchievements" :key="idx">
            <div class="achievement-item">
              <!-- 桌面端图片 -->
              <img class="achievement-img desktop-img" :src="item.image" :alt="item.title" />
              <!-- 移动端图片区域 -->
              <div class="achievement-image-wrapper">
                <img class="achievement-img mobile-img" :src="item.image" :alt="item.title" />
              </div>
              <!-- 移动端显示的文字区域 -->
              <div class="achievement-text">
                <div class="achievement-title">
                  {{ item.title }}<span v-if="item.author && item.author !== '待填写'" class="achievement-author"> - {{ item.author }}</span>
                </div>
                <div class="achievement-desc">{{ item.desc }}</div>
              </div>
              <!-- 桌面端遮罩 -->
              <a v-if="item.link" class="mask-link" :href="item.link" target="_blank" rel="noopener">
                <div class="achievement-mask">
                  <div class="achievement-title">
                    {{ item.title }}<span v-if="item.author && item.author !== '待填写'" class="achievement-author"> - {{ item.author }}</span>
                  </div>
                  <div class="achievement-desc">{{ item.desc }}</div>
                </div>
              </a>
              <div v-else class="achievement-mask">
                <div class="achievement-title">
                  {{ item.title }}<span v-if="item.author && item.author !== '待填写'" class="achievement-author"> - {{ item.author }}</span>
                </div>
                <div class="achievement-desc">{{ item.desc }}</div>
              </div>
            </div>
          </el-carousel-item>
        </el-carousel>
      </div>
    </div>

    <!-- 导师简介 -->
    <div class="page-section">
      <div class="section-title">
        <el-icon class="section-icon"><User /></el-icon>
        <span class="section-title-left">导师简介</span>
      </div>
      <div class="section-content">
        <MarkdownContent file="mentor-introduction.md" />
      </div>
    </div>

    <!-- 科研项目 -->
    <div class="page-section">
      <div class="section-title">
        <el-icon class="section-icon"><FolderOpened /></el-icon>
        <span class="section-title-left">科研项目</span>
        <span class="section-title-right" @click="goToProjects"> more > </span>
      </div>
      <div class="section-content">
        <MarkdownContent file="projects-preview.md" />
      </div>
    </div>

    <!-- 学术论文 -->
    <div class="page-section">
      <div class="section-title">
        <el-icon class="section-icon"><Document /></el-icon>
        <span class="section-title-left">学术论文</span>
        <span class="section-title-right" @click="goToPapers"> more > </span>
      </div>
      <div class="section-content">
        <MarkdownContent file="papers-preview.md" />
      </div>
    </div>

    <!-- 软著专利 -->
    <div class="page-section">
      <div class="section-title">
        <el-icon class="section-icon"><Trophy /></el-icon>
        <span class="section-title-left">软著专利</span>
        <span class="section-title-right" @click="goToSoftwritingPatent"> more > </span>
      </div>
      <div class="section-content">
        <MarkdownContent file="softwriting-patent-preview.md" />
      </div>
    </div>

    <!-- 获奖荣誉 -->
    <div class="page-section">
      <div class="section-title">
        <el-icon class="section-icon"><Medal /></el-icon>
        <span class="section-title-left">获奖荣誉</span>
        <span class="section-title-right" @click="goToHonour"> more > </span>
      </div>
      <div class="section-content">
        <MarkdownContent file="honour-preview.md" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'
import { OfficeBuilding, User, FolderOpened, Document, Trophy, Medal } from '@element-plus/icons-vue'
import MarkdownContent from '../MarkdownContent.vue'

// 解析 md 文本为数组
function parseTeamAchievements(mdText, baseUrl = '/') {
  const sections = mdText.split(/^## /m).filter(Boolean);
  return sections.map(block => {
    const lines = block.split('\n');
    const title = lines[0].trim();
    let image = '', link = '', author = '', desc = '';
    for (let i = 1; i < lines.length; ++i) {
      if (lines[i].startsWith('image:')) {
        image = lines[i].replace('image:', '').trim();
      } else if (lines[i].startsWith('link:')) {
        link = lines[i].replace('link:', '').trim();
      } else if (lines[i].startsWith('author:')) {
        author = lines[i].replace('author:', '').trim();
      } else if (lines[i].trim()) {
        desc += lines[i].trim() + ' ';
      }
    }
    // 修正根路径，图片拼 baseUrl
    if (image && !/^https?:\/\//.test(image)) {
      image = image.replace(/^public\//, ''); // 剥离 public 前缀
      image = `${baseUrl}${image.startsWith('/') ? image.slice(1) : image}`;
    }
    return { title, image, link, author, desc: desc.trim() };
  });
}

const teamAchievements = ref([])

// 响应式轮播图高度和触发方式
const isMobile = computed(() => {
  if (typeof window === 'undefined') return false
  return window.innerWidth <= 768
})

const carouselTrigger = computed(() => {
  return isMobile.value ? 'click' : 'hover'
})

function getBaseUrl() {
  const base = import.meta.env.BASE_URL || '/';
  return base.endsWith('/') ? base : `${base}/`;
}

onMounted(async () => {
  try {
    const baseUrl = getBaseUrl();
    const res = await fetch(`${baseUrl}content/team-achievements-preview.md`);
    const text = await res.text();
    teamAchievements.value = parseTeamAchievements(text, baseUrl);
  } catch (e) {
    teamAchievements.value = [];
  }
});

const props = defineProps({
  showPage: {
    type: Function,
    default: () => {}
  }
})

const goToProjects = () => {
  props.showPage('projects')
}

const goToPapers = () => {
  props.showPage('papers')
}

const goToSoftwritingPatent = () => {
  props.showPage('softwriting&patent')
}

const goToHonour = () => {
  props.showPage('honour')
}

const goToTeamAchievements = () => {
  props.showPage('teamAchievements')
}
</script>

<style scoped lang="scss">
.home-page {
  .page-section {
    background: rgba(255, 255, 255, 0.733);
    border: 1px solid rgb(227, 227, 227);
    margin-bottom: 25px;
    border-radius: 15px;

    .section-title {
      border-radius: 15px 15px 0 0;
      height: 70px;
      width: 100%;
      background: rgba(242, 248, 255, 0.608);
      display: flex;
      align-items: center;
      position: relative;
      padding: 0;

      .section-icon {
        width: 24px;
        height: 24px;
        margin: 0 15px;
        color: #409eff;
        font-size: 24px;
      }

      .section-title-left {
        line-height: 70px;
        font-size: 25px;
        font-weight: 700;
        margin: 0;
        display: inline-block;
        color: #333;
      }

      .section-title-right {
        display: inline-block;
        position: absolute;
        right: 5%;
        font-size: 14px;
        color: #409eff;
        cursor: pointer;

        &:hover {
          font-weight: 600;
          text-decoration: underline;
        }
      }
    }

    .section-content {
      padding: 20px;
      line-height: 30px;
      color: #666;
    }

    .achievements-carousel-wrapper {
      padding-top: 0;
      padding-bottom: 0;
      height: auto;
      overflow: visible;
    }
    .achievement-item {
      height: 450px;
      width: 100%;
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
    }
    .achievement-image-wrapper {
      display: none; // 桌面端不使用wrapper
    }
    .achievement-img.desktop-img {
      max-width: 100%;
      max-height: 100%;
      width: auto;
      height: 100%;
      object-fit: contain;
      display: block;
      margin: 0 auto;
      border-radius: 15px;
      background: #f4f6fa;
    }
    .achievement-img.mobile-img {
      display: none; // 桌面端隐藏移动端图片
    }
    .achievement-mask {
      transition: opacity 0.3s;
      opacity: 0;
      pointer-events: none;
      background: rgba(0, 0, 0, 0.4);
      backdrop-filter: blur(8px);
      -webkit-backdrop-filter: blur(8px);
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      color: #fff;
      border-radius: 15px;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      z-index: 2;
      padding: 30px;
      text-align: center;
      font-size: 18px;
    }
    .achievement-item:hover .achievement-mask {
      opacity: 1;
      pointer-events: auto;
    }
    // 移动端：文字显示在图片下方
    .achievement-text {
      display: none; // 桌面端隐藏，使用遮罩
    }
    .achievement-title {
      font-size: 24px;
      font-weight: 700;
      margin-bottom: 16px;
      text-shadow: 0 2px 8px rgba(0,0,0,0.22);
      
      .achievement-author {
        font-size: 18px;
        font-weight: 400;
      }
    }
    .achievement-desc {
      font-size: 16px;
      line-height: 1.7;
      text-shadow: 0 2px 6px rgba(0,0,0,0.15);
      word-break: break-word;
      margin: 0 auto;
      max-width: 80%;
    }
    // 轮播指示器样式优化
    :deep(.el-carousel__indicators) {
      margin-top: 20px;
      
      .el-carousel__indicator {
        padding: 8px;
        
        .el-carousel__button {
          width: 12px;
          height: 12px;
          border-radius: 50%;
          background-color: rgba(64, 158, 255, 0.4);
          transition: all 0.3s;
        }
        
        &.is-active .el-carousel__button {
          background-color: #409eff;
          width: 32px;
          border-radius: 6px;
        }
      }
    }
    // 轮播左右箭头按钮样式优化
    :deep(.el-carousel__arrow) {
      width: 48px;
      height: 48px;
      background-color: rgba(255, 255, 255, 0.95);
      border: 2px solid rgba(64, 158, 255, 0.6);
      box-shadow: 0 2px 12px rgba(0, 0, 0, 0.15);
      border-radius: 50%;
      transition: background-color 0.3s, border-color 0.3s, box-shadow 0.3s, color 0.3s;
      transform: none !important;
      opacity: 1 !important; // 默认显示
      
      .el-icon {
        font-size: 20px;
        color: #409eff;
        font-weight: bold;
      }
      
      &:hover {
        background-color: #409eff;
        border-color: #409eff;
        box-shadow: 0 4px 16px rgba(64, 158, 255, 0.4);
        transform: none !important;
        
        .el-icon {
          color: #fff;
        }
      }
    }
    .mask-link {
      display: contents;
    }
  }
}

// 移动端响应式样式
@media (max-width: 768px) {
  .home-page {
    .page-section {
      margin-bottom: 15px;
      border-radius: 10px;

      .section-title {
        height: 50px;
        border-radius: 10px 10px 0 0;

        .section-icon {
          width: 20px;
          height: 20px;
          margin: 0 10px;
          font-size: 20px;
        }

        .section-title-left {
          line-height: 50px;
          font-size: 18px;
        }

        .section-title-right {
          right: 15px;
          font-size: 13px;
          padding: 5px 0;
        }
      }

      .section-content {
        padding: 15px;
        line-height: 24px;
        font-size: 14px;
        height: auto !important;
        max-height: none !important;
        overflow: visible !important;
      }
    }

    // 轮播图移动端优化：参考TeamAchievementsPage的卡片式布局
    .achievements-carousel-wrapper {
      overflow: hidden !important;
      height: 360px !important;
      max-height: 360px !important;
      min-height: 360px !important;
      padding-bottom: 0 !important;
    }
    
    .achievement-item {
      height: 360px !important;
      min-height: 360px !important;
      max-height: 360px !important;
      flex-direction: column;
      background: rgba(248, 251, 255, 0.75);
      border: 1px solid rgba(227, 227, 227, 0.8);
      border-radius: 14px;
      overflow: hidden !important;
      display: flex !important;
      width: 100%;
      align-items: stretch !important;
      
      // 移动端隐藏桌面端图片
      .achievement-img.desktop-img {
        display: none !important;
      }
      
      // 移动端使用image-wrapper
      .achievement-image-wrapper {
        display: block !important;
        width: 100%;
        padding-top: 60%;
        position: relative;
        overflow: hidden;
        background: #f4f6f8;
        
        .achievement-img.mobile-img {
          display: block !important;
          position: absolute;
          top: 0;
          left: 0;
          width: 100%;
          height: 100%;
          object-fit: cover;
          border-radius: 0;
          background: transparent;
          margin: 0;
        }
      }
      
      // 移动端显示文字区域
      .achievement-text {
        display: flex !important;
        flex-direction: column;
        gap: 10px;
        padding: 18px;
        flex: 1;
        width: 100%;
        height: auto !important;
        max-height: none !important;
        min-height: auto !important;
        overflow: visible !important;
        
        .achievement-title {
          font-size: 18px;
          font-weight: 600;
          margin-bottom: 0;
          color: #1f2f3d;
          text-shadow: none;
          word-wrap: break-word;
          overflow-wrap: break-word;
          
          .achievement-author {
            font-size: 14px;
            font-weight: 400;
            color: #5c6b7a;
          }
        }
        
        .achievement-desc {
          font-size: 14px;
          line-height: 1.6;
          color: #5c6b7a;
          text-shadow: none;
          max-width: 100%;
          margin: 0;
          white-space: pre-line;
          word-wrap: break-word;
          overflow-wrap: break-word;
          height: auto !important;
          max-height: none !important;
        }
      }
      
      // 移动端隐藏遮罩
      .achievement-mask {
        display: none;
      }
    }

    // 移动端轮播图固定高度 - 与 App.vue 保持一致
    .team-achievements-carousel {
      height: 360px !important; 
      max-height: 360px !important;
      min-height: 360px !important;
      
      // 使用属性选择器覆盖内联样式
      &[style*="height"],
      &[style] {
        height: 360px !important;
        max-height: 360px !important;
        min-height: 360px !important;
      }
      
      :deep(.el-carousel__container) {
        height: 360px !important;
        min-height: 360px !important;
        max-height: 360px !important;
        overflow: hidden !important;
      }
      
      // 使用属性选择器覆盖内联样式
      :deep(.el-carousel__container[style*="height"]),
      :deep(.el-carousel__container[style]) {
        height: 360px !important;
        min-height: 360px !important;
        max-height: 360px !important;
        overflow: hidden !important;
      }
      
      :deep(.el-carousel__item) {
        height: 360px !important;
        min-height: 360px !important;
        max-height: 360px !important;
        display: flex !important;
        align-items: stretch !important;
        overflow: hidden !important;
      }
      
      // 使用属性选择器覆盖内联样式
      :deep(.el-carousel__item[style*="height"]),
      :deep(.el-carousel__item[style]),
      :deep(.el-carousel__item.is-active[style*="height"]),
      :deep(.el-carousel__item.is-active[style]),
      :deep(.el-carousel__item.is-animating[style*="height"]),
      :deep(.el-carousel__item.is-animating[style]) {
        height: 360px !important;
        min-height: 360px !important;
        max-height: 360px !important;
        display: flex !important;
        align-items: stretch !important;
        overflow: hidden !important;
      }
      
      // 确保轮播图内部的所有元素都能正常显示
      :deep(.el-carousel__item > *) {
        height: 100% !important;
        max-height: 100% !important;
        min-height: 100% !important;
        overflow: hidden !important;
      }
      
      // 移动端轮播按钮始终显示
      :deep(.el-carousel__arrow) {
        opacity: 1 !important;
        display: flex !important;
        width: 36px;
        height: 36px;
        
        .el-icon {
          font-size: 16px;
        }
      }
    }

    :deep(.el-carousel__indicators) {
      margin-top: 15px;
      
      .el-carousel__indicator {
        padding: 6px;
        
        .el-carousel__button {
          width: 8px;
          height: 8px;
        }
        
        &.is-active .el-carousel__button {
          width: 24px;
        }
      }
    }

    :deep(.el-carousel__arrow) {
      width: 36px;
      height: 36px;
      
      .el-icon {
        font-size: 16px;
      }
    }
  }
}
</style>

