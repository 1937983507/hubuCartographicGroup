<template>
  <div class="team-achievements-preview">
    <div v-if="loading" class="state-text">内容加载中...</div>
    <div v-else-if="error" class="state-text error">{{ error }}</div>
    <div v-else class="achievements-list">
      <a
        v-for="(card, index) in previewCards"
        :key="`${card.title}-${index}`"
        class="achievement-item"
        :href="card.link || '#'"
        target="_blank"
        rel="noopener noreferrer"
      >
        <div class="image-wrapper">
          <img :src="card.image" :alt="card.title" loading="lazy" />
        </div>
        <div class="content-wrapper">
          <h3>{{ card.title }}</h3>
          <p v-if="card.description">{{ card.description }}</p>
        </div>
      </a>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, computed } from 'vue'

const cards = ref([])
const loading = ref(true)
const error = ref('')

// 预览模式：只显示前2个项目
const previewCards = computed(() => {
  return cards.value.slice(0, 2)
})

const normalizeValue = line => line.split(':').slice(1).join(':').trim()

const resolveImageUrl = (raw) => {
  if (!raw) return ''
  // 绝对 URL 直接返回
  if (/^https?:\/\//i.test(raw)) return raw

  // 去掉前导的 public/，因为打包后会被提到根目录
  let path = raw.replace(/^public\//, '').replace(/^\/public\//, '')

  // 去掉开头的 /，后面用 BASE_URL 统一拼接
  path = path.replace(/^\//, '')

  const baseUrl = getBaseUrl()
  return `${baseUrl}${path}`
}

const parseMarkdown = text => {
  const lines = text.split('\n')
  const parsedCards = []
  let current = null

  lines.forEach(rawLine => {
    const line = rawLine.trim()

    if (line.startsWith('# ')) {
      return
    }

    if (line.startsWith('## ')) {
      if (current && current.title) {
        parsedCards.push(current)
      }
      current = {
        title: line.replace(/^##\s*/, '').trim(),
        image: '',
        link: '',
        description: ''
      }
      return
    }

    if (!current) return

    if (line.toLowerCase().startsWith('image:')) {
      current.image = resolveImageUrl(normalizeValue(line))
    } else if (line.toLowerCase().startsWith('link:')) {
      current.link = normalizeValue(line)
    } else if (line) {
      current.description = current.description
        ? `${current.description}\n${line}`
        : line
    }
  })

  if (current && current.title) {
    parsedCards.push(current)
  }

  cards.value = parsedCards
}

const getBaseUrl = () => {
  const base = import.meta.env.BASE_URL || '/'
  return base.endsWith('/') ? base : `${base}/`
}

const loadContent = async () => {
  loading.value = true
  error.value = ''

  try {
    const baseUrl = getBaseUrl()
    const url = `${baseUrl}content/team-achievements.md`
    const response = await fetch(url)
    if (!response.ok) {
      throw new Error(`加载失败（${response.status}）`)
    }
    const text = await response.text()
    parseMarkdown(text)
  } catch (err) {
    console.error(err)
    error.value = '内容加载失败，请稍后再试'
  } finally {
    loading.value = false
  }
}

onMounted(loadContent)
</script>

<style scoped lang="scss">
.team-achievements-preview {
  width: 100%;
}

.state-text {
  text-align: center;
  color: #888;
  font-size: 16px;
  padding: 40px 0;

  &.error {
    color: #f56c6c;
  }
}

.achievements-list {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.achievement-item {
  display: flex;
  gap: 20px;
  border: 1px solid rgba(227, 227, 227, 0.8);
  border-radius: 12px;
  padding: 15px;
  background: rgba(248, 251, 255, 0.5);
  text-decoration: none;
  color: inherit;
  transition: transform 0.25s ease, box-shadow 0.25s ease;

  &:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.08);
  }

  .image-wrapper {
    flex-shrink: 0;
    width: 200px;
    height: 150px;
    border-radius: 8px;
    overflow: hidden;
    background: #f4f6f8;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }

  .content-wrapper {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 10px;

    h3 {
      font-size: 18px;
      font-weight: 600;
      color: #1f2f3d;
      margin: 0;
      line-height: 1.4;
    }

    p {
      font-size: 14px;
      color: #5c6b7a;
      line-height: 1.6;
      margin: 0;
      white-space: pre-line;
    }
  }

  @media (max-width: 768px) {
    flex-direction: column;

    .image-wrapper {
      width: 100%;
      height: 200px;
    }
  }
}
</style>

