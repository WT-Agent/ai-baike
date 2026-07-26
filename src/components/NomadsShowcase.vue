<template>
  <section class="nomads-showcase-section">
    <div class="showcase-header">
      <div class="header-left">
        <h2 class="showcase-title">趣味百科与科普问答实战模板库</h2>
        <p class="showcase-subtitle">精选高频科学现象与历史冷知识探究，点击“一键套用”生成通俗解读</p>
      </div>
      <span class="showcase-badge">已收录 {{ showcaseItems.length }} 个科普主题</span>
    </div>

    <div class="showcase-grid">
      <div 
        v-for="item in showcaseItems" 
        :key="item.id" 
        class="glass-card showcase-card"
      >
        <div class="card-header">
          <span class="scenario-tag">{{ item.tag }}</span>
          <span class="usage-count">{{ item.usageCount }} 次探索</span>
        </div>

        <div class="card-content">
          <h3 class="item-title">{{ item.title }}</h3>
          <p class="item-prompt">“{{ item.prompt }}”</p>
        </div>

        <div class="card-action">
          <button class="apply-btn" @click="applyTemplate(item)">
            <span>一键套用</span>
            <svg class="arrow-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <line x1="5" y1="12" x2="19" y2="12"></line>
              <polyline points="12 5 19 12 12 19"></polyline>
            </svg>
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { computed } from 'vue';

const emit = defineEmits<{
  (e: 'apply-template', payload: { prompt: string; mode?: string; domain?: string }): void;
}>();

export interface ShowcaseItem {
  id: string;
  tag: string;
  title: string;
  prompt: string;
  mode?: string;
  domain?: string;
  usageCount: string;
}

const showcaseItems = computed<ShowcaseItem[]>(() => [
  {
    id: 'baike-1',
    tag: '宇宙物理',
    title: '黑洞形成与时间膨胀的通俗比喻',
    prompt: '为什么靠近黑洞时间会变慢？请用生动有趣的比喻向高中生解释引力透镜与时间膨胀效应。',
    mode: '通俗比喻与趣味解密',
    domain: '前沿科技与宇宙天文',
    usageCount: '45.8k'
  },
  {
    id: 'baike-2',
    tag: '量子力学',
    title: '量子纠缠幽灵作用的生动拆解',
    prompt: '如果向小朋友解释量子纠缠，为什么爱因斯坦称其为“幽灵般的超距作用”？用一双鞋子的比喻来做科普。',
    mode: '儿童少儿问答启蒙',
    domain: '前沿科技与宇宙天文',
    usageCount: '39.2k'
  },
  {
    id: 'baike-3',
    tag: '生物人体',
    title: '人体免疫系统对抗病毒的城堡战争',
    prompt: '把白细胞、T细胞与抗体比作防御城堡的兵种，详细拆解感冒时发烧与免疫反应的作战全过程。',
    mode: '通俗比喻与趣味解密',
    domain: '生物自然与人体奥秘',
    usageCount: '52.1k'
  },
  {
    id: 'baike-4',
    tag: '生活化学',
    title: '切洋葱流泪与雨后清香的化学奥秘',
    prompt: '为什么切洋葱时会让人流泪？以及下雨后空气中特别好闻的泥土清香（土臭素）到底是从哪里来的？',
    mode: '通俗比喻与趣味解密',
    domain: '生活物理与日常化学',
    usageCount: '31.6k'
  },
  {
    id: 'baike-5',
    tag: '历史冷知识',
    title: '胡椒比金子贵的古代香料战争冷知识',
    prompt: '为什么在中世纪欧洲胡椒能当货币用？揭秘地理大发现背后胡椒与香料贸易的搞笑历史冷知识。',
    mode: '短视频科普爆款文案',
    domain: '历史文明与考古冷知识',
    usageCount: '28.4k'
  },
  {
    id: 'baike-6',
    tag: '脑科学',
    title: '人为什么会产生“似曾相识”错觉',
    prompt: '从大脑海马体与神经元信号传输延迟角度，解释心理学和脑科学上的“既视感（Déjà vu）”现象。',
    mode: '学术严谨与原理解析',
    domain: '心理学与大脑神经',
    usageCount: '36.9k'
  }
]);

function applyTemplate(item: ShowcaseItem) {
  emit('apply-template', {
    prompt: item.prompt,
    mode: item.mode,
    domain: item.domain
  });
}
</script>

<style scoped>
.nomads-showcase-section {
  margin-top: 2rem;
  width: 100%;
}

.showcase-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 1.25rem;
  padding-bottom: 0.75rem;
  border-bottom: 1px solid var(--card-border);
}

.showcase-title {
  font-size: 1.2rem;
  font-weight: 700;
  color: var(--text-primary);
  background: var(--primary-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.showcase-subtitle {
  font-size: 0.825rem;
  color: var(--text-secondary);
  margin-top: 0.25rem;
}

.showcase-badge {
  font-size: 0.75rem;
  color: #a5b4fc;
  background: rgba(99, 102, 241, 0.12);
  border: 1px solid rgba(99, 102, 241, 0.25);
  padding: 4px 10px;
  border-radius: 20px;
}

.showcase-grid {
  display: grid;
  grid-template-columns: repeat(1, 1fr);
  gap: 1.25rem;
}

@media (min-width: 640px) {
  .showcase-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .showcase-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

.showcase-card {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
  padding: 1.25rem;
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid var(--card-border);
  border-radius: 14px;
  transition: all 0.25s ease;
}

.showcase-card:hover {
  background: rgba(255, 255, 255, 0.05);
  border-color: rgba(99, 102, 241, 0.4);
  transform: translateY(-3px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.4);
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.75rem;
}

.scenario-tag {
  font-size: 0.75rem;
  font-weight: 600;
  padding: 3px 8px;
  border-radius: 6px;
  background: rgba(168, 85, 247, 0.15);
  color: #c084fc;
  border: 1px solid rgba(168, 85, 247, 0.3);
}

.usage-count {
  font-size: 0.75rem;
  color: var(--text-secondary);
}

.card-content {
  margin-bottom: 1rem;
  flex: 1;
}

.item-title {
  font-size: 0.95rem;
  font-weight: 600;
  color: var(--text-primary);
  margin-bottom: 0.4rem;
}

.item-prompt {
  font-size: 0.825rem;
  color: var(--text-secondary);
  line-height: 1.45;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
  font-style: italic;
}

.card-action {
  padding-top: 0.75rem;
  border-top: 1px solid rgba(255, 255, 255, 0.04);
}

.apply-btn {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  padding: 0.5rem 1rem;
  background: rgba(99, 102, 241, 0.1);
  border: 1px solid rgba(99, 102, 241, 0.3);
  border-radius: 8px;
  color: #a5b4fc;
  font-size: 0.825rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
}

.showcase-card:hover .apply-btn {
  background: var(--primary-gradient);
  border-color: transparent;
  color: white;
}

.arrow-icon {
  width: 14px;
  height: 14px;
  transition: transform 0.2s ease;
}

.apply-btn:hover .arrow-icon {
  transform: translateX(3px);
}
</style>
