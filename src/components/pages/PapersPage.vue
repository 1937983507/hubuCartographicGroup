<template>
  <div class="papers-page">
    <!-- 移动端遮罩层 -->
    <div 
      v-if="isMobile && sidebarVisible" 
      class="sidebar-overlay"
      @click="closeSidebar">
    </div>
    
    <!-- 移动端浮动按钮 -->
    <el-badge 
      v-if="isMobile && activeFilterCount > 0" 
      :value="activeFilterCount" 
      :max="99"
      class="filter-badge-wrapper">
      <el-button
        type="primary"
        :icon="Filter"
        circle
        class="filter-toggle-button"
        @click="toggleSidebar">
      </el-button>
    </el-badge>
    <el-button
      v-else-if="isMobile"
      type="primary"
      :icon="Filter"
      circle
      class="filter-toggle-button"
      @click="toggleSidebar">
    </el-button>
    
    <!-- 左侧内容区域 -->
    <div class="page-section">
      <div class="section-title">
        <el-icon class="section-icon"><Document /></el-icon>
        <span class="section-title-left">学术论文</span>
      </div>
      
      <!-- 搜索栏 -->
      <div class="search-bar">
        <el-select
          v-model="searchType"
          placeholder="搜索类型"
          class="search-type-select"
          @change="handleSearch">
          <el-option label="篇名" value="title" />
          <el-option label="关键词" value="keywords" />
          <el-option label="摘要" value="abstract" />
        </el-select>
        <el-input
          v-model="searchKeyword"
          placeholder="请输入搜索关键词"
          clearable
          class="search-input"
          @input="handleSearch"
          @clear="handleSearch">
          <template #prefix>
            <el-icon><Search /></el-icon>
          </template>
        </el-input>
        <el-button
          type="primary"
          :icon="Search"
          @click="handleSearch"
          class="search-button">
          搜索
        </el-button>
        <el-button
          v-if="searchKeyword"
          :icon="RefreshLeft"
          @click="clearSearch"
          class="clear-search-button">
          清除
        </el-button>
      </div>
      
      <div class="section-content">
        <PapersContent 
          file="papers.md" 
          :sort-order="sortOrder"
          :selected-years="selectedYears"
          :selected-languages="selectedLanguages"
          :selected-types="selectedTypes"
          :selected-categories="selectedCategories"
          :search-keyword="searchKeyword"
          :search-type="searchType"
          @papers-loaded="handlePapersLoaded"
          @papers-data-loaded="handlePapersDataLoaded"
        />
      </div>
    </div>
    
    <!-- 右侧筛选工具栏 -->
    <div class="filter-sidebar" :class="{ 'sidebar-open': isMobile && sidebarVisible }">
        <div class="filter-header">
          <div class="filter-title-wrapper">
            <el-icon class="filter-icon"><Filter /></el-icon>
            <span class="filter-title">筛选条件</span>
          </div>
          <div class="filter-header-right">
            <div class="result-count-badge">
              <span class="result-count">共 {{ filteredCount }} 篇</span>
            </div>
            <!-- 移动端关闭按钮 -->
            <el-button
              v-if="isMobile"
              text
              :icon="Close"
              circle
              class="close-sidebar-button"
              @click="closeSidebar">
            </el-button>
          </div>
        </div>
        
        <el-collapse v-model="activeCollapseItems" class="filter-collapse">
          <!-- 排序 -->
          <el-collapse-item name="sort" class="filter-item">
            <template #title>
              <div class="collapse-title">
                <el-icon class="title-icon"><Sort /></el-icon>
                <span>排序</span>
              </div>
            </template>
            <div class="filter-item-content">
              <el-radio-group v-model="sortOrder" size="default" class="sort-radio-group">
                <el-radio-button label="desc">时间降序</el-radio-button>
                <el-radio-button label="asc">时间升序</el-radio-button>
              </el-radio-group>
            </div>
          </el-collapse-item>
          
          <!-- 年份筛选 -->
          <el-collapse-item name="year" class="filter-item">
            <template #title>
              <div class="collapse-title">
                <el-icon class="title-icon"><Calendar /></el-icon>
                <span>年份</span>
              </div>
            </template>
            <div class="filter-item-content">
              <el-button-group class="action-buttons">
                <el-button size="small" @click="selectedYears = [...availableYears]">全选</el-button>
                <el-button size="small" @click="selectedYears = []">全不选</el-button>
              </el-button-group>
              <el-checkbox-group v-model="selectedYears" size="default" class="checkbox-group-vertical">
                <el-checkbox 
                  v-for="year in availableYears"
                  :key="year"
                  :label="year"
                  class="filter-checkbox">
                  {{ year }}年<span class="count-badge">({{ getYearCount(year) }})</span>
                </el-checkbox>
              </el-checkbox-group>
            </div>
          </el-collapse-item>
          
          <!-- 语言筛选 -->
          <el-collapse-item name="language" class="filter-item">
            <template #title>
              <div class="collapse-title">
                <el-icon class="title-icon"><ChatDotRound /></el-icon>
                <span>语言</span>
              </div>
            </template>
            <div class="filter-item-content">
              <el-button-group class="action-buttons">
                <el-button size="small" @click="selectedLanguages = [...availableLanguages]">全选</el-button>
                <el-button size="small" @click="selectedLanguages = []">全不选</el-button>
              </el-button-group>
              <el-checkbox-group v-model="selectedLanguages" size="default" class="checkbox-group-vertical">
                <el-checkbox 
                  v-for="lang in availableLanguages" 
                  :key="lang" 
                  :label="lang"
                  class="filter-checkbox">
                  {{ lang }}<span class="count-badge">({{ getLanguageCount(lang) }})</span>
                </el-checkbox>
              </el-checkbox-group>
            </div>
          </el-collapse-item>
          
          <!-- 类型筛选 -->
          <el-collapse-item name="type" class="filter-item">
            <template #title>
              <div class="collapse-title">
                <el-icon class="title-icon"><DocumentCopy /></el-icon>
                <span>类型</span>
              </div>
            </template>
            <div class="filter-item-content">
              <el-button-group class="action-buttons">
                <el-button size="small" @click="selectedTypes = [...availableTypes]">全选</el-button>
                <el-button size="small" @click="selectedTypes = []">全不选</el-button>
              </el-button-group>
              <el-checkbox-group v-model="selectedTypes" size="default" class="checkbox-group-vertical">
                <el-checkbox 
                  v-for="type in availableTypes" 
                  :key="type" 
                  :label="type"
                  class="filter-checkbox">
                  {{ type }}<span class="count-badge">({{ getTypeCount(type) }})</span>
                </el-checkbox>
              </el-checkbox-group>
            </div>
          </el-collapse-item>
          
          <!-- 来源筛选 -->
          <el-collapse-item name="category" class="filter-item">
            <template #title>
              <div class="collapse-title">
                <el-icon class="title-icon"><Collection /></el-icon>
                <span>来源</span>
              </div>
            </template>
            <div class="filter-item-content">
              <el-button-group class="action-buttons">
                <el-button size="small" @click="selectedCategories = [...availableCategories]">全选</el-button>
                <el-button size="small" @click="selectedCategories = []">全不选</el-button>
              </el-button-group>
              <el-checkbox-group v-model="selectedCategories" size="default" class="checkbox-group-vertical">
                <el-checkbox 
                  v-for="cat in availableCategories" 
                  :key="cat" 
                  :label="cat"
                  class="filter-checkbox">
                  {{ cat }}<span class="count-badge">({{ getCategoryCount(cat) }})</span>
                </el-checkbox>
              </el-checkbox-group>
            </div>
          </el-collapse-item>
        </el-collapse>
        
        <div class="filter-actions">
          <el-button 
            type="default" 
            size="default" 
            @click="clearFilters" 
            class="clear-button"
            :icon="RefreshLeft">
            清除筛选
          </el-button>
        </div>
      </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { 
  Document, 
  Filter, 
  Sort, 
  Calendar, 
  ChatDotRound, 
  DocumentCopy, 
  Collection,
  RefreshLeft,
  Search,
  Close
} from '@element-plus/icons-vue'
import PapersContent from '../PapersContent.vue'

const sortOrder = ref('desc')
const selectedYears = ref([])
const selectedLanguages = ref([])
const selectedTypes = ref([])
const selectedCategories = ref([])

const availableYears = ref([])
const availableLanguages = ref([])
const availableTypes = ref([])
const availableCategories = ref([])
const filteredCount = ref(0)
const loading = ref(true)
const activeCollapseItems = ref(['sort']) // 默认只展开排序
const allPapers = ref([]) // 存储所有论文数据用于计算数量
const searchKeyword = ref('') // 搜索关键词
const searchType = ref('title') // 搜索类型：title(篇名)、keywords(关键词)、abstract(摘要)

// 移动端侧边栏控制
const isMobile = computed(() => {
  if (typeof window === 'undefined') return false
  return window.innerWidth <= 768
})
const sidebarVisible = ref(false)

// 计算激活的筛选条件数量
const activeFilterCount = computed(() => {
  let count = 0
  if (selectedYears.value.length > 0 && selectedYears.value.length < availableYears.value.length) count++
  if (selectedLanguages.value.length > 0 && selectedLanguages.value.length < availableLanguages.value.length) count++
  if (selectedTypes.value.length > 0 && selectedTypes.value.length < availableTypes.value.length) count++
  if (selectedCategories.value.length > 0 && selectedCategories.value.length < availableCategories.value.length) count++
  if (sortOrder.value !== 'desc') count++
  return count
})

const toggleSidebar = () => {
  sidebarVisible.value = !sidebarVisible.value
}

const closeSidebar = () => {
  sidebarVisible.value = false
}

// 监听窗口大小变化
const handleResize = () => {
  if (!isMobile.value) {
    sidebarVisible.value = false
  }
}

onMounted(() => {
  parsePapersForOptions()
  // 自动全选所有复选框
  setTimeout(() => {
    selectedYears.value = [...availableYears.value]
    selectedLanguages.value = [...availableLanguages.value]
    selectedTypes.value = [...availableTypes.value]
    selectedCategories.value = [...availableCategories.value]
  }, 500) // 等待数据加载
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
})

const getBaseUrl = () => {
  const base = import.meta.env.BASE_URL || '/'
  return base.endsWith('/') ? base : `${base}/`
}

// 解析论文数据以获取可用选项
const parsePapersForOptions = async () => {
  try {
    const baseUrl = getBaseUrl()
    const url = `${baseUrl}content/papers.md`
    const response = await fetch(url)
    if (!response.ok) return
    const text = await response.text()
    const sections = text.split(/^---$/m).map(s => s.trim()).filter(s => s)
    
    const years = new Set()
    const languages = new Set()
    const types = new Set()
    const categories = new Set()
    
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
      // 收集categories，将无分类的归为“无”
      if (paper.categories) {
        if (Array.isArray(paper.categories)) {
          let found = false
          paper.categories.forEach(c => {
            if (c) {
              found = true
              categories.add(c)
            }
          })
          if (!found) categories.add('无')
        } else if (typeof paper.categories === 'string' && paper.categories) {
          paper.categories.split(/[、,]/).map(c => c.trim()).forEach(c => c && categories.add(c))
        } else {
          categories.add('无')
        }
      } else {
        categories.add('无')
      }
    })
    
    availableYears.value = Array.from(years).sort((a, b) => parseInt(b) - parseInt(a))
    availableLanguages.value = Array.from(languages).sort()
    availableTypes.value = Array.from(types).sort()
    availableCategories.value = Array.from(categories).sort()
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

const handlePapersDataLoaded = (papers) => {
  allPapers.value = papers
}

// 计算各筛选项的论文数量
const getYearCount = (year) => {
  return allPapers.value.filter(paper => String(paper.year) === year).length
}

const getLanguageCount = (language) => {
  return allPapers.value.filter(paper => paper.language === language).length
}

const getTypeCount = (type) => {
  return allPapers.value.filter(paper => paper.type === type).length
}

const getCategoryCount = (category) => {
  return allPapers.value.filter(paper => {
    let cats = []
    if (!paper.categories || (Array.isArray(paper.categories) && paper.categories.length === 0)) {
      cats = ['无']
    } else if (Array.isArray(paper.categories)) {
      cats = paper.categories.length > 0 ? paper.categories : ['无']
    } else if (typeof paper.categories === 'string') {
      cats = paper.categories.split(/[、,]/).map(c => c.trim()).filter(Boolean)
      if (cats.length === 0) cats = ['无']
    }
    return cats.includes(category)
  }).length
}

const clearFilters = () => {
  selectedYears.value = []
  selectedLanguages.value = []
  selectedTypes.value = []
  selectedCategories.value = []
  sortOrder.value = 'desc'
}

const handleSearch = () => {
  // 搜索逻辑在 PapersContent 组件中实现
  // 这里只需要触发更新即可
}

const clearSearch = () => {
  searchKeyword.value = ''
  handleSearch()
}

</script>

<style scoped lang="scss">
.papers-page {
  width: 100%;
  max-width: 100%;
  margin-left: -30px; // 整体往左靠
  position: relative;

  .page-section {
    width: 100%;
    background: rgba(255, 255, 255, 0.733);
    border: 1px solid rgb(227, 227, 227);
    margin-bottom: 25px;
    border-radius: 15px;
    box-sizing: border-box;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.08);

    .section-title {
      border-radius: 15px 15px 0 0;
      height: 70px;
      width: 100%;
      background: linear-gradient(135deg, rgba(242, 248, 255, 0.8) 0%, rgba(235, 245, 255, 0.9) 100%);
      display: flex;
      align-items: center;
      position: relative;
      padding: 0;
      border-bottom: 1px solid rgba(227, 227, 227, 0.5);

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

    .search-bar {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 16px 20px;
      background: rgba(248, 249, 250, 0.6);
      border-bottom: 1px solid rgba(227, 227, 227, 0.5);

      .search-input {
        flex: 1;
        max-width: 400px;
      }

      .search-type-select {
        width: 120px;
      }

      .search-button {
        flex-shrink: 0;
      }

      .clear-search-button {
        flex-shrink: 0;
      }
    }

    .section-content {
      padding: 20px;
      line-height: 30px;
      color: #666;
      box-sizing: border-box;
      overflow-wrap: break-word;
      word-wrap: break-word;
    }
    
    .no-results {
      padding: 40px 20px;
      text-align: center;
    }
  }

  .filter-sidebar {
    width: 300px;
    background: rgba(255, 255, 255, 0.733);
    border: 1px solid rgb(227, 227, 227);
    border-radius: 12px;
    padding: 16px;
    height: fit-content;
    position: absolute;
    top: 0;
    left: calc(100% + 20px); // 位于page-section右侧，添加20px间距
    max-height: calc(100vh - 120px);
    overflow-y: auto;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.08);
    transition: all 0.3s ease;
    z-index: 10;

    &:hover {
      box-shadow: 0 6px 20px rgba(0, 0, 0, 0.12);
    }

    .filter-header {
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      align-items: center;
      gap: 10px;
      margin-bottom: 14px;
      padding-bottom: 12px;
      border-bottom: 2px solid #e8e8e8;

      .filter-title-wrapper {
        display: flex;
        align-items: center;
        gap: 8px;

        .filter-icon {
          font-size: 18px;
          color: #409eff;
        }

        .filter-title {
          font-weight: 600;
          color: #333;
          font-size: 18px;
          letter-spacing: 0.5px;
        }
      }

      .filter-header-right {
        display: flex;
        align-items: center;
        gap: 10px;
      }

      .result-count-badge {
        background: linear-gradient(135deg, #409eff 0%, #66b1ff 100%);
        color: white;
        padding: 6px 12px;
        border-radius: 20px;
        display: inline-block;
        flex-shrink: 0;

        .result-count {
          font-size: 13px;
          font-weight: 500;
        }
      }

      .close-sidebar-button {
        color: #666;
        font-size: 18px;
        
        &:hover {
          color: #409eff;
        }
      }
    }

    .filter-collapse {
      border: none;
      background: transparent;

      :deep(.el-collapse-item) {
        background: #ffffff;
        border: 1px solid #e8e8e8;
        border-radius: 8px;
        margin-bottom: 8px;
        overflow: hidden;
        transition: all 0.3s ease;

        &:hover {
          border-color: #409eff;
          box-shadow: 0 2px 8px rgba(64, 158, 255, 0.15);
        }

        &:last-child {
          margin-bottom: 0;
        }

        .el-collapse-item__header {
          padding: 10px 14px;
          font-weight: 500;
          color: #333;
          font-size: 14px;
          border-bottom: none;
          height: auto;
          line-height: 1.5;
          background: rgba(248, 249, 250, 0.6);
          transition: all 0.3s ease;

          &:hover {
            background: rgba(64, 158, 255, 0.08);
            color: #409eff;
          }

          .collapse-title {
            display: flex;
            align-items: center;
            gap: 10px;

            .title-icon {
              font-size: 16px;
              color: #409eff;
            }

            span {
              font-weight: 500;
            }
          }
        }

        .el-collapse-item__content {
          padding: 12px 14px;
          background: #ffffff;
        }

        &.is-active {
          border-color: #409eff;
          box-shadow: 0 2px 12px rgba(64, 158, 255, 0.2);
        }
      }
    }

    .filter-item-content {
      .action-buttons {
        width: 100%;
        margin-bottom: 10px;
        display: flex;
        gap: 6px;

        .el-button {
          flex: 1;
          border-radius: 6px;
          transition: all 0.3s ease;

          &:hover {
            transform: translateY(-1px);
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
          }
        }
      }

      .checkbox-group-vertical {
        display: flex;
        flex-direction: column;
        gap: 4px;

        .filter-checkbox {
          padding: 6px 10px;
          border-radius: 6px;
          transition: all 0.2s ease;
          margin: 0;

          &:hover {
            background: rgba(64, 158, 255, 0.06);
          }

          :deep(.el-checkbox__label) {
            font-size: 13px;
            color: #606266;
            padding-left: 6px;
            display: flex;
            align-items: center;
            gap: 6px;
          }

          :deep(.el-checkbox__input.is-checked .el-checkbox__inner) {
            background-color: #409eff;
            border-color: #409eff;
          }

          .count-badge {
            color: #909399;
            font-size: 12px;
            font-weight: 500;
            margin-left: 2px;
          }
        }
      }

      .sort-radio-group {
        width: 100%;
        display: flex;

        :deep(.el-radio-button) {
          flex: 1;

          .el-radio-button__inner {
            width: 100%;
            border-radius: 6px;
            transition: all 0.3s ease;
          }
        }
      }
    }

    .filter-actions {
      margin-top: 14px;
      padding-top: 12px;
      border-top: 2px solid #e8e8e8;

      .clear-button {
        width: 100%;
        border-radius: 8px;
        font-weight: 500;
        transition: all 0.3s ease;
        background: linear-gradient(135deg, #f5f7fa 0%, #e9ecef 100%);
        border: 1px solid #d3d4d6;

        &:hover {
          background: linear-gradient(135deg, #e9ecef 0%, #dee2e6 100%);
          transform: translateY(-1px);
          box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        }

        &:active {
          transform: translateY(0);
        }
      }
    }

    // 自定义滚动条
    &::-webkit-scrollbar {
      width: 6px;
    }

    &::-webkit-scrollbar-track {
      background: #f1f1f1;
      border-radius: 10px;
    }

    &::-webkit-scrollbar-thumb {
      background: #c1c1c1;
      border-radius: 10px;

      &:hover {
        background: #a8a8a8;
      }
    }
  }

  // 移动端浮动按钮（仅在移动端显示）
  .filter-toggle-button {
    display: none; // 默认隐藏，在移动端媒体查询中显示
  }

  .filter-badge-wrapper {
    display: none; // 默认隐藏，在移动端媒体查询中显示
  }

  // 移动端遮罩层（仅在移动端显示）
  .sidebar-overlay {
    display: none; // 默认隐藏，在移动端媒体查询中显示
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }

  @keyframes slideIn {
    from {
      transform: translateX(-100%);
    }
    to {
      transform: translateX(0);
    }
  }

  // 响应式设计
  @media (max-width: 1024px) {
    margin-left: 0;

    .filter-sidebar {
      width: 100%;
      position: relative;
      top: 0;
      left: auto;
      max-height: none;
      // margin-top: 20px;
    }
  }

  // 移动端优化
  @media (max-width: 768px) {
    // 显示移动端浮动按钮
    .filter-toggle-button,
    .filter-badge-wrapper {
      display: flex !important;
      position: fixed;
      bottom: 80px;
      right: 20px;
      z-index: 1000;
    }

    .filter-toggle-button {
      width: 50px;
      height: 50px;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
      transition: all 0.3s ease;
      font-size: 20px;

      :deep(.el-icon) {
        font-size: 20px;
      }

      &:hover {
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
      }

      &:active {
        transform: translateY(0);
      }
    }

    .filter-badge-wrapper {
      :deep(.el-badge__content) {
        top: -4px;
        right: -4px;
      }
    }

    // 显示移动端遮罩层
    .sidebar-overlay {
      display: block !important;
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0, 0, 0, 0.5);
      z-index: 998;
      animation: fadeIn 0.3s ease;
    }

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

      .search-bar {
        flex-direction: column;
        gap: 10px;
        padding: 12px 15px;

        .search-type-select {
          width: 100%;
        }

        .search-input {
          max-width: 100%;
        }

        .search-button,
        .clear-search-button {
          width: 100%;
        }
      }

      .section-content {
        padding: 15px;
        line-height: 24px;
        font-size: 14px;
      }
    }

    .filter-sidebar {
      position: fixed;
      top: 70px; // 考虑 header 高度，避免被遮挡
      left: 0;
      width: 85%;
      max-width: 320px;
      height: calc(100vh - 70px); // 减去 header 高度
      max-height: calc(100vh - 70px);
      padding: 16px;
      z-index: 999;
      transform: translateX(-100%);
      transition: transform 0.3s ease;
      border-radius: 0;
      box-shadow: 2px 0 12px rgba(0, 0, 0, 0.15);
      overflow-y: auto;
      overflow-x: hidden;

      &.sidebar-open {
        transform: translateX(0);
        animation: slideIn 0.3s ease;
      }

      .filter-header {
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
        gap: 10px;
        position: sticky;
        top: 0;
        background: rgba(255, 255, 255, 0.95);
        backdrop-filter: blur(10px);
        z-index: 10;
        margin-bottom: 14px;
        padding: 12px 0;
        padding-top: 0; // 移除顶部 padding，因为侧边栏已经有 top: 70px
        border-bottom: 2px solid #e8e8e8;

        .filter-title-wrapper {
          .filter-title {
            font-size: 16px;
          }
        }

        .filter-header-right {
          display: flex;
          align-items: center;
          gap: 8px;
        }

        .result-count-badge {
          padding: 4px 10px;

          .result-count {
            font-size: 12px;
          }
        }
      }

      .filter-collapse {
        :deep(.el-collapse-item__header) {
          padding: 8px 12px;
          font-size: 13px;
        }

        :deep(.el-collapse-item__content) {
          padding: 10px 12px;
        }
      }

      .filter-item-content {
        .action-buttons {
          flex-direction: column;
          gap: 8px;

          .el-button {
            width: 100%;
          }
        }

        .checkbox-group-vertical {
          .filter-checkbox {
            padding: 8px 10px;

            :deep(.el-checkbox__label) {
              font-size: 12px;
            }
          }
        }
      }
    }
  }
}
</style>

