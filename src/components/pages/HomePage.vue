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

    <!-- 团队成果 -->
    <div class="page-section">
      <div class="section-title">
        <el-icon class="section-icon"><Trophy /></el-icon>
        <span class="section-title-left">团队成果</span>
        <span class="section-title-right" @click="goToTeamAchievements">
          more >
        </span>
      </div>
      <div class="section-content">
        <MarkdownContent file="team-achievements-preview.md" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { OfficeBuilding, User, FolderOpened, Document, Trophy, Medal } from '@element-plus/icons-vue'
import MarkdownContent from '../MarkdownContent.vue'

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
  }
}
</style>

