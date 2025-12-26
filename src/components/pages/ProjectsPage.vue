<template>
  <div class="projects-page">
    <div class="page-section">
      <div class="section-title">
        <el-icon class="section-icon"><FolderOpened /></el-icon>
        <span class="section-title-left">科研项目</span>
      </div>
      <div class="section-content">
        <MarkdownContent file="projects.md" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { FolderOpened } from '@element-plus/icons-vue'
import MarkdownContent from '../MarkdownContent.vue'
</script>

<style scoped lang="scss">
.projects-page {
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
    }

    .section-content {
      padding: 20px;
      line-height: 30px;
      color: #666;
    }
  }
}

// 移动端响应式样式
@media (max-width: 768px) {
  .projects-page {
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
      }

      .section-content {
        padding: 15px;
        line-height: 24px;
        font-size: 14px;
      }
    }
  }
}
</style>

