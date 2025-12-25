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
        <el-carousel height="450px" trigger="hover" indicator-position="outside" v-if="teamAchievements.length">
          <el-carousel-item v-for="(item, idx) in teamAchievements" :key="idx">
            <div class="achievement-item">
              <img class="achievement-img" :src="item.image" alt="" />
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
import { ref, onMounted } from 'vue'
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
    .achievement-img {
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
</style>

