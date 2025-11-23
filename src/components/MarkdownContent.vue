<template>
  <div class="markdown-content" v-html="htmlContent"></div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { marked } from 'marked'

const props = defineProps({
  file: {
    type: String,
    required: true
  }
})

const htmlContent = ref('')
const loading = ref(true)

// 配置 marked
marked.setOptions({
  breaks: true,
  gfm: true
})

onMounted(async () => {
  try {
    const response = await fetch(`/content/${props.file}`)
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }
    const text = await response.text()
    htmlContent.value = marked(text)
  } catch (error) {
    console.error('Failed to load markdown file:', error)
    htmlContent.value = '<p>内容加载失败</p>'
  } finally {
    loading.value = false
  }
})
</script>

<style scoped lang="scss">
.markdown-content {
  :deep(p) {
    margin-bottom: 8px;
    line-height: 1.8;
  }

  :deep(a) {
    color: #409eff;
    text-decoration: none;

    &:hover {
      text-decoration: underline;
    }
  }

  :deep(b) {
    font-weight: bold;
  }

  :deep(div) {
    margin-bottom: 8px;
  }

  :deep(h2) {
    font-weight: 600;
    font-size: 20px;
    color: #333;
    margin: 20px 0 10px 0;
    padding-bottom: 10px;
    border-bottom: 1px solid rgb(224, 224, 224);
  }

  :deep(hr) {
    margin: 20px 0;
    border: none;
    border-bottom: 1px solid rgb(224, 224, 224);
  }

  :deep(a) {
    color: #409eff;
    text-decoration: none;
    font-size: 16px;
    display: block;
    margin-bottom: 8px;

    &:hover {
      text-decoration: underline;
    }
  }

  :deep(p) {
    margin-bottom: 8px;
    line-height: 1.8;
  }
}
</style>

