<template>
  <router-view />
</template>

<script setup>
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

/* 移动端抽屉样式覆盖 - 覆盖 Element Plus 默认样式 */
.mobile-drawer .el-drawer__header {
  margin-bottom: 0 !important;
}

.mobile-drawer .el-drawer__body {
  padding-top: 0 !important;
}

@media (max-width: 768px) {
  /* 轮播图容器 - 固定高度 */
  .team-achievements-carousel[style*="height"],
  .team-achievements-carousel[style],
  .team-achievements-carousel {
    height: 360px !important;
    max-height: 360px !important;
    min-height: 360px !important;
  }
  
  /* 轮播图内部容器 - 固定高度 */
  .team-achievements-carousel .el-carousel__container[style*="height"],
  .team-achievements-carousel .el-carousel__container[style],
  .team-achievements-carousel .el-carousel__container {
    height: 360px !important;
    min-height: 360px !important;
    max-height: 360px !important;
    overflow: hidden !important;
  }
  
  /* 轮播图项目 - 固定高度 */
  .team-achievements-carousel .el-carousel__item[style*="height"],
  .team-achievements-carousel .el-carousel__item[style],
  .team-achievements-carousel .el-carousel__item,
  .team-achievements-carousel .el-carousel__item.is-active[style*="height"],
  .team-achievements-carousel .el-carousel__item.is-active[style],
  .team-achievements-carousel .el-carousel__item.is-active,
  .team-achievements-carousel .el-carousel__item.is-animating[style*="height"],
  .team-achievements-carousel .el-carousel__item.is-animating[style],
  .team-achievements-carousel .el-carousel__item.is-animating {
    height: 360px !important;
    min-height: 360px !important;
    max-height: 360px !important;
    display: flex !important;
    align-items: stretch !important;
    overflow: hidden !important;
  }
}
</style>

