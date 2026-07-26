<template>
  <div class="glass-card ticker-card">
    <div class="ticker-wrapper">
      <div v-for="(item, index) in tickerItems" :key="index" class="ticker-item">
        <span>{{ item.text }}</span>
        <span style="color: var(--success-color); font-weight: bold;">[科普解答完成]</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const tickerItems = ref([
  { text: '用户 136****8291 解锁了黑洞形成与时间膨胀的生动物理比喻' },
  { text: '用户 178****4910 解答了人体为什么会打嗝与神经反射机制' },
  { text: '用户 181****0392 生成了量子纠缠通俗比喻少儿科普文案' },
  { text: '用户 133****7721 探究了中世纪胡椒比黄金还贵的历史经济学冷知识' },
  { text: '用户 159****8234 解析了猫咪咕噜声的身体自我愈合机制' },
]);
</script>
