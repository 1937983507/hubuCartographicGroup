<template>
  <div class="team-achievements-page">
    <div class="page-section">
      <div class="section-title">
        <el-icon class="section-icon"><Trophy /></el-icon>
        <span class="section-title-left">{{ heading }}</span>
      </div>
      <div class="section-content">
        <div v-if="loading" class="state-text">内容加载中...</div>
        <div v-else-if="error" class="state-text error">{{ error }}</div>
        <div v-else class="achievements-grid">
          <a
            v-for="(card, index) in cards"
            :key="`${card.title}-${index}`"
            class="achievement-card"
            :href="card.link || '#'"
            target="_blank"
            rel="noopener noreferrer"
          >
            <div class="image-wrapper">
              <img :src="card.image" :alt="card.title" loading="lazy" />
            </div>
            <div class="card-body">
              <h3>{{ card.title }}</h3>
              <p v-if="card.description">{{ card.description }}</p>
            </div>
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import { Trophy } from '@element-plus/icons-vue'

const heading = ref('团队成果')
const cards = ref([])
const loading = ref(true)
const error = ref('')

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
      heading.value = line.replace(/^#\s*/, '')
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
.team-achievements-page {
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
      color: #444;
    }
  }
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

.achievements-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 20px;

  @media (max-width: 768px) {
    grid-template-columns: 1fr;
  }
}

.achievement-card {
  border: 1px solid rgba(227, 227, 227, 0.8);
  border-radius: 14px;
  overflow: hidden;
  background: rgba(248, 251, 255, 0.75);
  text-decoration: none;
  color: inherit;
  transition: transform 0.25s ease, box-shadow 0.25s ease;
  display: flex;
  flex-direction: column;
  min-height: 320px;

  &:hover {
    transform: translateY(-4px);
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.08);
  }

  .image-wrapper {
    width: 100%;
    padding-top: 60%;
    position: relative;
    overflow: hidden;
    background: #f4f6f8;

    img {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }

  .card-body {
    padding: 18px;
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 10px;

    h3 {
      font-size: 18px;
      font-weight: 600;
      color: #1f2f3d;
      margin: 0;
    }

    p {
      font-size: 14px;
      color: #5c6b7a;
      line-height: 1.6;
      margin: 0;
      white-space: pre-line;
    }
  }
}
</style>

