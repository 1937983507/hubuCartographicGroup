<template>
  <div class="team-achievements-preview">
    <div v-if="loading" class="state-text">内容加载中...</div>
    <div v-else-if="error" class="state-text error">{{ error }}</div>
    <div v-else-if="cards.length > 0" class="carousel-container">
      <el-carousel
        :interval="5000"
        :arrow="cards.length > 1 ? 'hover' : 'never'"
        height="650px"
        indicator-position="outside"
        class="achievements-carousel"
      >
        <el-carousel-item v-for="(card, index) in cards" :key="`${card.title}-${index}`">
          <a
            class="achievement-item"
            :href="card.link || '#'"
            target="_blank"
            rel="noopener noreferrer"
          >
            <div class="image-wrapper">
              <img 
                :src="card.image" 
                :alt="card.title" 
                loading="lazy" 
              />
            </div>
            <div class="content-wrapper">
              <h3>{{ card.title }}</h3>
              <p v-if="card.description">{{ card.description }}</p>
            </div>
          </a>
        </el-carousel-item>
      </el-carousel>
    </div>
    <div v-else class="state-text">暂无内容</div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'

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
      return
    }

    if (line.startsWith('## ')) {
      if (current && current.title) {
        parsedCards.push(current)
      }
      current = {
        title: line.replace(/^##\s*/, '').trim(),
        image: '',
        gif: '',
        link: '',
        description: ''
      }
      return
    }

    if (!current) return

    if (line.toLowerCase().startsWith('image:')) {
      current.image = resolveImageUrl(normalizeValue(line))
    } else if (line.toLowerCase().startsWith('gif:')) {
      current.gif = resolveImageUrl(normalizeValue(line))
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
    const url = `${baseUrl}content/team-achievements-preview.md`
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

.carousel-container {
  width: 100%;
  // padding: 10px 0;
}

:deep(.achievements-carousel) {
  width: 100% !important;
  display: block !important;

  .el-carousel__container {
    width: 100% !important;
    height: 480px !important;
  }

  .el-carousel__item {
    width: 100% !important;
    padding: 0 5px;
    box-sizing: border-box;
    display: flex !important;
    justify-content: center !important;
    align-items: center !important;

    > a {
      width: 100%;
      display: block;
    }
  }

  .el-carousel__wrapper {
    width: 100% !important;
  }

  .el-carousel__indicators {
    margin-top: 15px;
  }

  .el-carousel__indicator {
    button {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background-color: rgba(64, 158, 255, 0.4);
      transition: all 0.3s ease;
    }

    &.is-active button {
      background-color: #409eff;
      width: 24px;
      border-radius: 4px;
    }
  }

  .el-carousel__arrow {
    background-color: rgba(255, 255, 255, 0.9);
    border: 1px solid rgba(227, 227, 227, 0.8);
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;

    &:hover {
      background-color: #409eff;
      border-color: #409eff;
      color: #fff;
    }
  }
}

.achievement-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 25px;
  width: 100%;
  max-width: 800px;
  // height: 100%;
  margin: 0 auto;
  border: none;
  border-radius: 0;
  padding: 0px 20px;
  background: transparent;
  text-decoration: none;
  color: inherit;
  box-shadow: none;
  box-sizing: border-box;

  .image-wrapper {
    width: 100%;
    max-width: 750px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0 auto;

    img {
      width: 100%;
      max-width: 750px;
      max-height: 600px;
      height: auto;
      object-fit: contain;
      border-radius: 8px;
      display: block;
    }
  }

  .content-wrapper {
    width: 100%;
    max-width: 750px;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    gap: 6px;
    flex-shrink: 0;

    h3 {
      font-size: 22px;
      font-weight: 600;
      color: #1f2f3d;
      margin: 0;
      line-height: 1.4;
      transition: color 0.3s ease;
    }

    p {
      font-size: 15px;
      color: #5c6b7a;
      line-height: 1.7;
      margin: 0;
      white-space: pre-line;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
      text-align: center;
    }
  }

  &:hover .content-wrapper h3 {
    color: #409eff;
  }

  @media (max-width: 768px) {
    padding: 20px;

    .image-wrapper {
      img {
        max-height: 300px;
      }
    }

    .content-wrapper {
      h3 {
        font-size: 18px;
      }

      p {
        font-size: 14px;
        -webkit-line-clamp: 3;
        line-clamp: 3;
        text-align: center;
      }
    }
  }
}

:deep(.achievements-carousel .el-carousel__container) {
  @media (max-width: 768px) {
    height: 420px !important;
  }
}
</style>

