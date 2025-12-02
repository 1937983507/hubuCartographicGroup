<template>
  <div ref="contentRef" class="markdown-content" v-html="htmlContent"></div>
</template>

<script setup>
import { ref, onMounted, watch, nextTick } from 'vue'
import { marked } from 'marked'

const props = defineProps({
  file: {
    type: String,
    required: true
  }
})

const htmlContent = ref('')
const loading = ref(true)
const contentRef = ref(null)

// 配置 marked
marked.setOptions({
  breaks: true,
  gfm: true
})

const collator = new Intl.Collator('zh-Hans-CN', {
  numeric: true,
  sensitivity: 'base'
})

const sortTable = (table, columnIndex, order) => {
  if (!table) return

  const hasTbody = table.tBodies && table.tBodies.length > 0
  const body = hasTbody ? table.tBodies[0] : table
  const rows = Array.from(body.rows)

  // If there is no tbody, skip the first row (header)
  const dataRows = hasTbody ? rows : rows.slice(1)

  dataRows.sort((a, b) => {
    const aText = a.cells[columnIndex]?.innerText.trim() || ''
    const bText = b.cells[columnIndex]?.innerText.trim() || ''
    const result = collator.compare(aText, bText)
    return order === 'asc' ? result : -result
  })

  dataRows.forEach(row => {
    body.appendChild(row)
  })
}

const enhanceTables = () => {
  if (!contentRef.value) return

  const tables = contentRef.value.querySelectorAll('table')
  tables.forEach(table => {
    const headerRow =
      table.querySelector('thead tr') || table.querySelector('tr')
    if (!headerRow) return

    const headers = headerRow.querySelectorAll('th')
    headers.forEach((th, index) => {
      th.style.cursor = 'pointer'
      th.dataset.sortOrder = 'none'

      th.addEventListener('click', () => {
        const currentOrder = th.dataset.sortOrder === 'asc' ? 'desc' : 'asc'
        headers.forEach(header => {
          header.dataset.sortOrder = 'none'
          header.classList.remove('sorted-asc', 'sorted-desc')
        })
        th.dataset.sortOrder = currentOrder
        th.classList.add(
          currentOrder === 'asc' ? 'sorted-asc' : 'sorted-desc'
        )
        sortTable(table, index, currentOrder)
      })
    })

    const gradeHeader = Array.from(headers).find(header =>
      header.innerText.includes('年级')
    )
    if (gradeHeader) {
      const gradeIndex = Array.from(headers).indexOf(gradeHeader)
      gradeHeader.dataset.sortOrder = 'asc'
      gradeHeader.classList.add('sorted-asc')
      sortTable(table, gradeIndex, 'asc')
    }
  })
}

const getBaseUrl = () => {
  const base = import.meta.env.BASE_URL || '/'
  return base.endsWith('/') ? base : `${base}/`
}

const loadMarkdown = async () => {
  try {
    const baseUrl = getBaseUrl()
    const url = `${baseUrl}content/${props.file}`
    const response = await fetch(url)
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
}

onMounted(loadMarkdown)

watch(
  htmlContent,
  async () => {
    await nextTick()
    enhanceTables()
  },
  { immediate: false }
)
</script>

<style scoped lang="scss">
.markdown-content {
  width: 100%;
  max-width: 100%;
  overflow-wrap: break-word;
  word-wrap: break-word;

  :deep(p) {
    margin-bottom: 8px;
    line-height: 1.8;
    white-space: normal;
    word-wrap: break-word;
    overflow-wrap: break-word;
    max-width: 100%;
  }

  :deep(a) {
    color: #409eff;
    text-decoration: none;
    display: inline;
    word-break: break-word;
    overflow-wrap: break-word;
    max-width: 100%;

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

  // 论文链接样式 - 段落中的链接
  :deep(p > a) {
    color: #409eff;
    text-decoration: none;
    font-size: 16px;
    font-weight: 500;
    display: inline-block;
    margin-bottom: 8px;
    transition: color 0.3s;

    &:hover {
      color: #66b1ff;
      text-decoration: underline;
    }
  }

  :deep(hr) {
    margin: 20px 0;
    border: none;
    border-bottom: 1px solid rgb(224, 224, 224);
  }

  :deep(table) {
    width: 100%;
    border-collapse: collapse;
    margin: 15px 0;
    font-size: 15px;
    text-align: left;
    background: transparent;
  }

  :deep(th),
  :deep(td) {
    border: 1px solid rgba(224, 224, 224, 0.8);
    padding: 10px 12px;
    color: #444;
    background: transparent;
  }

  :deep(th) {
    font-weight: 600;
    color: #222;
    position: relative;
  }

  :deep(tr:nth-child(even) td) {
    background: transparent;
  }

  :deep(th::after) {
    content: '⇅';
    font-size: 12px;
    color: #bbb;
    margin-left: 6px;
    position: relative;
    top: -1px;
    transition: color 0.2s, transform 0.2s;
  }

  :deep(th.sorted-asc::after) {
    content: '↑';
    color: #409eff;
  }

  :deep(th.sorted-desc::after) {
    content: '↓';
    color: #409eff;
  }
}
</style>
