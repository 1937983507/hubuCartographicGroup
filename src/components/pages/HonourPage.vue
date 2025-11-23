<template>
  <div class="honour-page">
    <div class="page-section">
      <div class="section-title">
        <el-icon class="section-icon"><Medal /></el-icon>
        <span class="section-title-left">获奖荣誉</span>
      </div>
      <div class="section-content">
        <MarkdownContent file="honour.md" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { Medal } from '@element-plus/icons-vue'
import MarkdownContent from '../MarkdownContent.vue'
</script>

<style scoped lang="scss">
.honour-page {
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
</style>

