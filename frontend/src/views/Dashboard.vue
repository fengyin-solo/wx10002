<template>
  <section class="page">
    <header class="page-head">
      <div>
        <h2>运营概览</h2>
        <p class="page-desc">汇总各业务模块的关键指标，先看总量再看异常。</p>
      </div>
    </header>
    <div class="stat-row">
      <article v-for="card in cards" :key="card.label" class="stat-card">
        <span class="stat-label">{{ card.label }}</span>
        <strong class="stat-value">{{ card.value }}</strong>
      </article>
    </div>
    <table class="data-table">
      <thead>
        <tr><th>业务模块</th><th>今日新增</th><th>待处理</th><th>异常量</th></tr>
      </thead>
      <tbody>
        <tr v-for="row in moduleRows" :key="row.name">
          <td>{{ row.name }}</td>
          <td>{{ row.created }}</td>
          <td>{{ row.pending }}</td>
          <td>{{ row.abnormal }}</td>
        </tr>
      </tbody>
    </table>
  </section>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'

import { fetchJson } from '@/api/client'

type Overview = {
  cards: { label: string; value: number }[]
  modules: { name: string; created: number; pending: number; abnormal: number }[]
}

const cards = ref<Overview['cards']>([])
const moduleRows = ref<Overview['modules']>([])

onMounted(async () => {
  try {
    const payload = await fetchJson<Overview>('/api/overview')
    cards.value = payload.cards
    moduleRows.value = payload.modules
  } catch {
    cards.value = [{"label": "业务模块", "value": 0}, {"label": "今日新增", "value": 0}]
    moduleRows.value = [{"name": "电站档案", "created": 0, "pending": 0, "abnormal": 0}, {"name": "巡检计划", "created": 0, "pending": 0, "abnormal": 0}, {"name": "组件清洗", "created": 0, "pending": 0, "abnormal": 0}, {"name": "逆变器管理", "created": 0, "pending": 0, "abnormal": 0}, {"name": "发电监测", "created": 0, "pending": 0, "abnormal": 0}, {"name": "故障处置", "created": 0, "pending": 0, "abnormal": 0}, {"name": "备件管理", "created": 0, "pending": 0, "abnormal": 0}, {"name": "箱变管理", "created": 0, "pending": 0, "abnormal": 0}, {"name": "开关站管理", "created": 0, "pending": 0, "abnormal": 0}, {"name": "关口计量", "created": 0, "pending": 0, "abnormal": 0}, {"name": "气象监测", "created": 0, "pending": 0, "abnormal": 0}, {"name": "并网调度", "created": 0, "pending": 0, "abnormal": 0}, {"name": "电缆线路", "created": 0, "pending": 0, "abnormal": 0}, {"name": "安防巡视", "created": 0, "pending": 0, "abnormal": 0}, {"name": "定期检修", "created": 0, "pending": 0, "abnormal": 0}, {"name": "汇流箱管理", "created": 0, "pending": 0, "abnormal": 0}, {"name": "能效分析", "created": 0, "pending": 0, "abnormal": 0}, {"name": "安全培训", "created": 0, "pending": 0, "abnormal": 0}]
  }
})
</script>
