<template>
  <div class="papers-content">
    <div v-if="loading" class="loading-state">
      <p>加载中...</p>
    </div>
    <div v-else-if="filteredAndSortedPapers.length === 0" class="no-papers">
      <el-empty description="没有找到符合条件的论文" />
    </div>
    <div v-else>
      <div v-for="(paper, index) in filteredAndSortedPapers" :key="index" class="paper-item">
      <!-- 标题 -->
      <h2 class="paper-title">
        <span class="paper-number">{{ index + 1 }}.</span>
        <a :href="paper.link" target="_blank" class="title-link">{{ paper.title }}</a>
      </h2>

      <!-- 论文信息 -->
      <div class="paper-info">
        <!-- 类型标签 -->
        <span class="paper-type">[{{ paper.type }}]</span>

        <!-- 作者 -->
        <span class="authors">
          <template v-for="(author, authorIndex) in paper.authors" :key="authorIndex">
            <span 
              :class="['author', { 'highlight-author': isHighlightAuthor(author) }]"
            >
              {{ author }}
            </span>
            <span v-if="authorIndex < paper.authors.length - 1" class="author-separator">&nbsp;</span>
          </template>
        </span>

        <!-- 期刊 -->
        <span class="journal">-《{{ paper.journal }}》</span>

        <!-- 分类标签 -->
        <span v-if="paper.categories && paper.categories.length > 0" class="categories">
          <span 
            v-for="(category, catIndex) in paper.categories" 
            :key="catIndex"
            class="category-tag"
          >
            {{ category }}
          </span>
        </span>

        <!-- 时间 -->
        <span class="publish-date">{{ formatDate(paper) }}</span>
      </div>

      <!-- 摘要 -->
      <div v-if="paper.abstract" class="abstract-section">
        <span class="abstract-label">摘要:</span><span class="abstract-text">{{ paper.abstract }}</span>
      </div>
      <!-- 关键词 -->
      <div v-if="paper.keywords" class="keywords-section">
        <span class="keywords-label">关键词：</span><span class="keywords">{{ Array.isArray(paper.keywords) ? paper.keywords.join('、') : paper.keywords }}</span>
      </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const props = defineProps({
  file: {
    type: String,
    required: true
  },
  sortOrder: {
    type: String,
    default: 'desc' // 'asc' 或 'desc'
  },
  selectedYears: {
    type: Array,
    default: () => []
  },
  selectedLanguages: {
    type: Array,
    default: () => []
  },
  selectedTypes: {
    type: Array,
    default: () => []
  },
  selectedCategories: {
    type: Array,
    default: () => []
  }
})

const papers = ref([])
const loading = ref(true)

const emit = defineEmits(['papers-loaded'])

// 计算筛选和排序后的论文列表
const filteredAndSortedPapers = computed(() => {
  let result = [...papers.value]
  
  // 年份筛选
  if (props.selectedYears.length > 0) {
    result = result.filter(paper => {
      const year = paper.year ? String(paper.year) : ''
      return props.selectedYears.includes(year)
    })
  }
  
  // 语言筛选
  if (props.selectedLanguages.length > 0) {
    result = result.filter(paper => {
      const language = paper.language || ''
      return props.selectedLanguages.includes(language)
    })
  }
  
  // 类型筛选
  if (props.selectedTypes.length > 0) {
    result = result.filter(paper => {
      const type = paper.type || ''
      return props.selectedTypes.includes(type)
    })
  }
  
  // 分类(categories)筛选
  if (props.selectedCategories && props.selectedCategories.length > 0) {
    result = result.filter(paper => {
      let cats = []
      if (!paper.categories || (Array.isArray(paper.categories) && paper.categories.length === 0)) {
        cats = ['无']
      } else if (Array.isArray(paper.categories)) {
        cats = paper.categories.length > 0 ? paper.categories : ['无']
      } else if (typeof paper.categories === 'string') {
        cats = paper.categories.split(/[、,]/).map(c => c.trim()).filter(Boolean)
        if (cats.length === 0) cats = ['无']
      }
      return cats.some(cat => props.selectedCategories.includes(cat))
    })
  }
  
  // 排序
  result.sort((a, b) => {
    const yearA = parseInt(a.year) || 0
    const yearB = parseInt(b.year) || 0
    
    if (props.sortOrder === 'asc') {
      return yearA - yearB
    } else {
      return yearB - yearA
    }
  })
  
  return result
})

// 监听筛选结果变化，通知父组件
watch([filteredAndSortedPapers, loading], ([newValue, isLoading]) => {
  if (!isLoading) {
    emit('papers-loaded', newValue.length)
  }
}, { immediate: true })

// 判断是否是高亮作者（成晓强或Cheng Xiaoqiang等变体）
const isHighlightAuthor = (author) => {
  const highlightNames = ['成晓强', 'Cheng Xiaoqiang', 'CHENG X', 'Cheng X', 'Cheng Xiaoqiang*', '成晓强*', '成晓强***']
  return highlightNames.some(name => author.includes(name) || name.includes(author))
}

// 格式化日期显示
const formatDate = (paper) => {
  if (paper.issue) {
    return `${paper.year}年${paper.issue}`
  }
  if (paper.date) {
    // 如果date包含年份信息，提取年份
    const yearMatch = paper.date.match(/(\d{4})/)
    if (yearMatch) {
      return paper.date
    }
    return `${paper.year}年`
  }
  return `${paper.year}年`
}

// 解析YAML格式的论文数据
const parsePapers = (text) => {
  const papers = []
  // 按 --- 分割，过滤空部分
  const sections = text.split(/^---$/m).map(s => s.trim()).filter(s => s)
  
  sections.forEach(section => {
    const lines = section.trim().split('\n')
    const paper = {}
    
    let currentKey = null
    let currentValue = []
    let inArray = false
    
    lines.forEach(line => {
      const trimmed = line.trim()
      if (!trimmed) {
        // 空行时，如果是数组模式，继续；否则结束当前键
        if (!inArray && currentKey) {
          // 结束多行文本
          if (paper[currentKey] && typeof paper[currentKey] === 'string') {
            paper[currentKey] = paper[currentKey].trim()
          }
        }
        return
      }
      
      // 检查是否是数组项（以 - 开头，且前面有空格或行首）
      if (/^\s*-\s+/.test(line) || trimmed.startsWith('- ')) {
        if (!inArray && currentKey) {
          // 保存之前的非数组值
          if (paper[currentKey] && typeof paper[currentKey] === 'string') {
            paper[currentKey] = paper[currentKey].trim()
          }
        }
        if (!inArray) {
          currentValue = []
          inArray = true
        }
        const value = trimmed.replace(/^-\s+/, '').trim()
        if (value) {
          currentValue.push(value)
        }
      } else if (trimmed.includes(':')) {
        // 保存之前的数组或值
        if (inArray && currentKey) {
          paper[currentKey] = currentValue.length === 1 ? currentValue[0] : [...currentValue]
          currentValue = []
          inArray = false
        } else if (currentKey && !inArray && paper[currentKey]) {
          paper[currentKey] = paper[currentKey].trim()
        }
        
        const colonIndex = trimmed.indexOf(':')
        currentKey = trimmed.substring(0, colonIndex).trim()
        const value = trimmed.substring(colonIndex + 1).trim()
        
        if (value) {
          paper[currentKey] = value
          currentValue = []
          inArray = false
        } else {
          // 空值，可能是数组或多行文本
          paper[currentKey] = ''
          currentValue = []
          inArray = false // 默认不是数组，除非下一行是 - 开头
        }
      } else if (currentKey) {
        if (inArray) {
          // 继续数组项（不应该到这里，因为数组项应该以 - 开头）
          currentValue.push(trimmed)
        } else {
          // 多行文本值
          if (paper[currentKey]) {
            paper[currentKey] += ' ' + trimmed
          } else {
            paper[currentKey] = trimmed
          }
        }
      }
    })
    
    // 保存最后的数组或值
    if (inArray && currentKey) {
      paper[currentKey] = currentValue.length === 1 ? currentValue[0] : [...currentValue]
    } else if (currentKey && paper[currentKey] && typeof paper[currentKey] === 'string') {
      paper[currentKey] = paper[currentKey].trim()
    }
    
    // 确保authors是数组
    if (paper.authors && !Array.isArray(paper.authors)) {
      // 尝试分割作者（支持多种分隔符）
      paper.authors = paper.authors.split(/[,;，；]\s*/).map(a => a.trim()).filter(a => a)
    }
    
    // 确保categories是数组
    if (paper.categories && !Array.isArray(paper.categories)) {
      paper.categories = paper.categories.split(/[、,]\s*/).map(c => c.trim()).filter(c => c)
    }
    
    if (Object.keys(paper).length > 0) {
      papers.push(paper)
    }
  })
  
  return papers
}

onMounted(async () => {
  try {
    const response = await fetch(`/content/${props.file}`)
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }
    const text = await response.text()
    papers.value = parsePapers(text)
  } catch (error) {
    console.error('Failed to load papers file:', error)
    papers.value = []
  } finally {
    loading.value = false
  }
})
</script>

<style scoped lang="scss">
.papers-content {
  width: 100%;
  max-width: 100%;

  .loading-state,
  .no-papers {
    padding: 40px 20px;
    text-align: center;
    color: #666;
  }

  .paper-item {
    margin-bottom: 32px;
    padding-bottom: 24px;
    border-bottom: 1px solid #e8e8e8;

    &:last-child {
      border-bottom: none;
      margin-bottom: 0;
    }

    .paper-title {
      font-size: 18px;
      font-weight: 600;
      margin: 0 0 14px 0;
      display: flex;
      align-items: flex-start;
      gap: 8px;
      line-height: 1.6;

      .paper-number {
        font-weight: 600;
        color: #333;
        flex-shrink: 0;
        margin-top: 2px;
      }

      .title-link {
        color: #2c3e50;
        text-decoration: none;
        flex: 1;
        transition: all 0.3s ease;
        line-height: 1.6;
        font-size: 20px;
        font-weight: bold;
        display: inline-block;

        &:hover {
          color: #409eff;
          text-decoration: underline;
        }
      }
    }

    .paper-info {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      gap: 10px;
      margin-bottom: 14px;
      font-size: 14px;
      line-height: 1.7;
      color: #555;

      .paper-type {
        color: #666;
        font-weight: 500;
        font-size: 13px;
      }

      .authors {
        display: inline-flex;
        flex-wrap: wrap;
        align-items: center;
        gap: 4px;

        .author {
          color: #666;
          font-weight: 400;

          &.highlight-author {
            color: #000;
            font-weight: 700;
          }
        }

        .author-separator {
          color: #666;
        }
      }

      .journal {
        color: #666;
        font-style: normal;
      }

      .categories {
        display: inline-flex;
        flex-wrap: wrap;
        gap: 6px;

        .category-tag {
          padding: 3px 10px;
          border: 1px solid #e0e0e0;
          border-radius: 12px;
          font-size: 12px;
          color: #666;
          background-color: #f8f9fa;
          transition: all 0.2s ease;

          &:hover {
            border-color: #409eff;
            background-color: #ecf5ff;
            color: #409eff;
          }
        }
      }

      .publish-date {
        color: #333;
        font-weight: 400;
      }
    }

    .abstract-section {
      margin-bottom: 12px;
      line-height: 1.75;
      font-size: 14px;
      color: #555;
      display: flex;
      align-items: flex-start;
      .abstract-label {
        font-weight: 600;
        color: #333;
        margin-right: 4px;
        white-space: nowrap;
        flex-shrink: 0;
      }
      .abstract-text {
        color: #555;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        line-clamp: 2;
        -webkit-box-orient: vertical;
        overflow: hidden;
        text-overflow: ellipsis;
        word-break: break-all;
        overflow-wrap: anywhere;
        white-space: normal;
        flex: 1;
      }
    }

    .keywords-section {
      margin-top: 10px;
      font-size: 13px;
      color: #666;
      display: flex;
      align-items: flex-start;
      .keywords-label {
        font-weight: 600;
        color: #333;
        margin-right: 4px;
        white-space: nowrap;
        flex-shrink: 0;
      }
      .keywords {
        word-spacing: 10px;
        letter-spacing: 0.3px;
      }
    }
  }
}
</style>
