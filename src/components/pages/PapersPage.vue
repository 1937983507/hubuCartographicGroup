<template>
  <div class="papers-page">
    <div class="page-section">
      <div class="section-title">
        <el-icon class="section-icon"><Document /></el-icon>
        <span class="section-title-left">学术论文</span>
      </div>
      
      <!-- 筛选工具栏 -->
      <div class="filter-toolbar">
        <div class="filter-group">
          <span class="filter-label">排序:</span>
          <el-radio-group v-model="sortOrder" size="small">
            <el-radio-button label="desc">时间降序</el-radio-button>
            <el-radio-button label="asc">时间升序</el-radio-button>
          </el-radio-group>
        </div>
        
        <div class="filter-group">
          <span class="filter-label">年份:</span>
          <el-checkbox-group v-model="selectedYears" size="small">
            <el-checkbox 
              v-for="year in availableYears" 
              :key="year" 
              :label="year"
            >
              {{ year }}年
            </el-checkbox>
          </el-checkbox-group>
        </div>
        
        <div class="filter-group">
          <span class="filter-label">语言:</span>
          <el-checkbox-group v-model="selectedLanguages" size="small">
            <el-checkbox 
              v-for="lang in availableLanguages" 
              :key="lang" 
              :label="lang"
            >
              {{ lang }}
            </el-checkbox>
          </el-checkbox-group>
        </div>
        
        <div class="filter-group">
          <span class="filter-label">类型:</span>
          <el-checkbox-group v-model="selectedTypes" size="small">
            <el-checkbox 
              v-for="type in availableTypes" 
              :key="type" 
              :label="type"
            >
              {{ type }}
            </el-checkbox>
          </el-checkbox-group>
        </div>
        
        <div class="filter-actions">
          <el-button size="small" @click="clearFilters">清除筛选</el-button>
          <span class="result-count">共 {{ filteredCount }} 篇</span>
        </div>
      </div>
      
      <div class="section-content">
        <PapersContent 
          file="papers.md" 
          :sort-order="sortOrder"
          :selected-years="selectedYears"
          :selected-languages="selectedLanguages"
          :selected-types="selectedTypes"
          @papers-loaded="handlePapersLoaded"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { Document } from '@element-plus/icons-vue'
import PapersContent from '../PapersContent.vue'

const sortOrder = ref('desc')
const selectedYears = ref([])
const selectedLanguages = ref([])
const selectedTypes = ref([])

const availableYears = ref([])
const availableLanguages = ref([])
const availableTypes = ref([])
const filteredCount = ref(0)
const loading = ref(true)

// 解析论文数据以获取可用选项
const parsePapersForOptions = async () => {
  try {
    const response = await fetch('/content/papers.md')
    if (!response.ok) return
    const text = await response.text()
    const sections = text.split(/^---$/m).map(s => s.trim()).filter(s => s)
    
    const years = new Set()
    const languages = new Set()
    const types = new Set()
    
    sections.forEach(section => {
      const lines = section.trim().split('\n')
      const paper = {}
      
      let currentKey = null
      let currentValue = []
      let inArray = false
      
      lines.forEach(line => {
        const trimmed = line.trim()
        if (!trimmed) return
        
        if (/^\s*-\s+/.test(line) || trimmed.startsWith('- ')) {
          if (!inArray) {
            currentValue = []
            inArray = true
          }
          const value = trimmed.replace(/^-\s+/, '').trim()
          if (value) currentValue.push(value)
        } else if (trimmed.includes(':')) {
          if (inArray && currentKey) {
            paper[currentKey] = currentValue.length === 1 ? currentValue[0] : [...currentValue]
            currentValue = []
            inArray = false
          }
          
          const colonIndex = trimmed.indexOf(':')
          currentKey = trimmed.substring(0, colonIndex).trim()
          const value = trimmed.substring(colonIndex + 1).trim()
          
          if (value) {
            paper[currentKey] = value
            currentValue = []
            inArray = false
          } else {
            paper[currentKey] = ''
            currentValue = []
            inArray = false
          }
        } else if (currentKey) {
          if (inArray) {
            currentValue.push(trimmed)
          } else {
            paper[currentKey] = (paper[currentKey] || '') + ' ' + trimmed
          }
        }
      })
      
      if (inArray && currentKey) {
        paper[currentKey] = currentValue.length === 1 ? currentValue[0] : [...currentValue]
      }
      
      if (paper.year) years.add(String(paper.year))
      if (paper.language) languages.add(paper.language)
      if (paper.type) types.add(paper.type)
    })
    
    availableYears.value = Array.from(years).sort((a, b) => parseInt(b) - parseInt(a))
    availableLanguages.value = Array.from(languages).sort()
    availableTypes.value = Array.from(types).sort()
  } catch (error) {
    console.error('Failed to load papers for options:', error)
  }
}

const handlePapersLoaded = (count) => {
  filteredCount.value = count
  if (count > 0) {
    loading.value = false
  }
}

const clearFilters = () => {
  selectedYears.value = []
  selectedLanguages.value = []
  selectedTypes.value = []
  sortOrder.value = 'desc'
}

onMounted(() => {
  parsePapersForOptions()
})
</script>

<style scoped lang="scss">
.papers-page {
  width: 100%;
  max-width: 100%;

  .page-section {
    background: rgba(255, 255, 255, 0.733);
    border: 1px solid rgb(227, 227, 227);
    margin-bottom: 25px;
    border-radius: 15px;
    width: 100%;
    max-width: 100%;
    box-sizing: border-box;

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

    .filter-toolbar {
      padding: 20px;
      background: rgba(248, 249, 250, 0.8);
      border-bottom: 1px solid rgb(227, 227, 227);
      
      .filter-group {
        margin-bottom: 16px;
        display: flex;
        align-items: flex-start;
        flex-wrap: wrap;
        gap: 12px;
        
        &:last-of-type {
          margin-bottom: 0;
        }
        
        .filter-label {
          font-weight: 600;
          color: #333;
          font-size: 14px;
          min-width: 50px;
          padding-top: 4px;
        }
      }
      
      .filter-actions {
        margin-top: 16px;
        padding-top: 16px;
        border-top: 1px solid #e8e8e8;
        display: flex;
        align-items: center;
        justify-content: space-between;
        
        .result-count {
          color: #666;
          font-size: 14px;
          font-weight: 500;
        }
      }
    }
    
    .no-results {
      padding: 40px 20px;
      text-align: center;
    }

    .section-content {
      padding: 20px;
      line-height: 30px;
      color: #666;
      width: 100%;
      max-width: 100%;
      box-sizing: border-box;
      overflow-wrap: break-word;
      word-wrap: break-word;
    }
  }
}
</style>

