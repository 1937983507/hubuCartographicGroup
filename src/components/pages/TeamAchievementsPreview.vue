<template>
  <div class="team-achievements-preview">
    <div v-if="loading" class="state-text">内容加载中...</div>
    <div v-else-if="error" class="state-text error">{{ error }}</div>
    <div v-else-if="cards.length > 0" class="carousel-container">
      <el-carousel
        :interval="5000"
        :arrow="cards.length > 1 ? 'hover' : 'never'"
        :height="carouselHeight"
        indicator-position="outside"
        class="achievements-carousel"
      >
        <el-carousel-item v-for="(card, index) in cards" :key="`${card.title}-${index}`">
          <a
            class="achievement-item"
            :href="card.link || '#'"
            target="_blank"
            rel="noopener noreferrer"
            @mouseenter="hoveredIndex = index"
            @mouseleave="hoveredIndex = -1"
          >
            <div class="image-wrapper">
              <img 
                :src="card.image" 
                :alt="card.title" 
                loading="lazy" 
              />
              <div 
                class="image-overlay"
                :class="{ 'is-visible': hoveredIndex === index }"
              >
                <div class="overlay-content">
                  <h3 class="overlay-title">{{ card.title }}</h3>
                  <p class="overlay-description">{{ card.description }}</p>
                </div>
              </div>
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
const hoveredIndex = ref(-1)
const carouselHeight = ref('470px')

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
    height: 400px !important;
  }

  .el-carousel__item {
    width: 100% !important;
    padding: 0 5px;
    box-sizing: border-box;
    display: flex !important;
    justify-content: center !important;
    align-items: flex-start !important;

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
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
  border: none;
  border-radius: 0;
  padding: 0;
  background: transparent;
  text-decoration: none;
  color: inherit;
  box-shadow: none;
  box-sizing: border-box;

  .image-wrapper {
    width: 100%;
    max-width: 750px;
    display: flex;
    align-items: flex-start;
    justify-content: center;
    margin: 0 auto;
    position: relative;
    border-radius: 8px;
    overflow: hidden;

    img {
      width: 100%;
      max-width: 750px;
      max-height: 580px;
      height: auto;
      object-fit: contain;
      border-radius: 8px;
      display: block;
      transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .image-overlay {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: linear-gradient(
        to bottom,
        rgba(0, 0, 0, 0.7) 0%,
        rgba(0, 0, 0, 0.5) 50%,
        rgba(0, 0, 0, 0.7) 100%
      );
      backdrop-filter: blur(4px);
      display: flex;
      align-items: center;
      justify-content: center;
      opacity: 0;
      visibility: hidden;
      transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
      border-radius: 8px;
      padding: 40px 30px;
      box-sizing: border-box;

      &.is-visible {
        opacity: 1;
        visibility: visible;
      }

      .overlay-content {
        text-align: center;
        color: #fff;
        max-width: 100%;
        width: 100%;

        .overlay-title {
          font-size: 28px;
          font-weight: 700;
          margin: 0 0 20px 0;
          line-height: 1.3;
          color: #fff;
          text-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
          transform: translateY(20px);
          opacity: 0;
          transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1) 0.1s;
        }

        .overlay-description {
          font-size: 16px;
          line-height: 1.8;
          margin: 0;
          color: rgba(255, 255, 255, 0.95);
          text-shadow: 0 1px 4px rgba(0, 0, 0, 0.15);
          transform: translateY(20px);
          opacity: 0;
          transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1) 0.2s;
          white-space: pre-line;
          max-height: 300px;
          overflow-y: auto;
          padding-right: 10px;

          &::-webkit-scrollbar {
            width: 6px;
          }

          &::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 3px;
          }

          &::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.3);
            border-radius: 3px;

            &:hover {
              background: rgba(255, 255, 255, 0.5);
            }
          }
        }
      }

      &.is-visible {
        .overlay-title,
        .overlay-description {
          transform: translateY(0);
          opacity: 1;
        }
      }
    }

    &:hover {
      img {
        transform: scale(1.05);
      }
    }
  }


  @media (max-width: 768px) {
    padding: 20px;

    .image-wrapper {
      img {
        max-height: 300px;
      }

      .image-overlay {
        padding: 30px 20px;

        .overlay-content {
          .overlay-title {
            font-size: 22px;
            margin-bottom: 15px;
          }

          .overlay-description {
            font-size: 14px;
            line-height: 1.6;
            max-height: 200px;
          }
        }
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

