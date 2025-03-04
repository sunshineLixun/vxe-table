<template>
  <div>
    <vxe-grid v-bind="gridOptions" :merge-cells="getMergeCells()"></vxe-grid>
  </div>
</template>

<script lang="ts" setup>
import { reactive } from 'vue'
import type { VxeGridProps } from '../../../types'

interface RowVO {
  id: number;
  name: string;
  role: string;
  sex: string;
  age: number;
  address: string;
}

const gridOptions = reactive<VxeGridProps<RowVO>>({
  border: true,
  showOverflow: true,
  showFooter: true,
  height: 600,
  scrollY: {
    enabled: true,
    gt: 0
  },
  scrollX: {
    enabled: true,
    gt: 0
  },
  mergeFooterItems: [{ row: 0, col: 1, rowspan: 2, colspan: 1 }],
  columns: Array.from({ length: 100 }, (_, index) => ({
    field: `col${index}`,
    title: `Column ${index}`,
    minWidth: 100
  })),
  data: []
})

// 模拟行数据
const loadList = (size = 200) => {
  const dataList: RowVO[] = []
  for (let i = 0; i < size; i++) {
    dataList.push({
      id: 10000 + i,
      name: 'Test' + i,
      role: 'Developer',
      sex: '男',
      age: 20,
      address: 'Address abc'
    })
  }
  gridOptions.data = dataList
}

loadList(500)

function getMergeCells () {
  const totalRow = 1000
  const mergeCells = []
  let currentRow = 0
  while (currentRow < totalRow) {
    const rowspan = Math.floor(Math.random() * 5) + 6
    const actualRowspan = Math.min(rowspan, totalRow - currentRow)

    mergeCells.push({
      row: currentRow,
      col: 1,
      rowspan: actualRowspan,
      colspan: 2
    })

    currentRow += actualRowspan
  }
  return mergeCells
}
</script>
