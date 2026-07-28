<template>
  <div class="app-container static-baike-container">
    <!-- 成功提示 -->
    <div v-if="copied" class="top-success-toast">
      已成功复制到剪贴板
    </div>

    <header>
      <h1>{{ appTitle }}</h1>
      <p>智能 AI 科普与工具选型系统 · 洞察人工智能演进脉络</p>
    </header>

    <!-- 顶部分类控制区 (Card Tabs) -->
    <nav class="card-tabs navigation-tabs">
      <button 
        class="tab-btn" 
        :class="{ active: activeTab === 'timeline' }"
        @click="activeTab = 'timeline'"
      >
        AI 历史进程
      </button>
      <button 
        class="tab-btn" 
        :class="{ active: activeTab === 'landscape' }"
        @click="activeTab = 'landscape'"
      >
        主流大模型地图
      </button>
      <button 
        class="tab-btn" 
        :class="{ active: activeTab === 'tools' }"
        @click="activeTab = 'tools'"
      >
        核心工具推荐
      </button>
      <button 
        class="tab-btn" 
        :class="{ active: activeTab === 'glossary' }"
        @click="activeTab = 'glossary'"
      >
        术语百科字典
      </button>
    </nav>

    <!-- 主展示面板 -->
    <main class="main-content-panel">
      
      <!-- 1. AI 历史进程面板 -->
      <section v-if="activeTab === 'timeline'" class="glass-card timeline-panel">
        <h2 class="panel-section-title">人工智能演进史</h2>
        <p class="panel-section-desc">从图灵测试的哲学萌芽，到超大规模深度神经网络的狂飙突进，回顾人工智能历史的关键节点。</p>
        
        <div class="vertical-timeline">
          <div 
            v-for="(item, index) in timelineData" 
            :key="index"
            class="timeline-item"
          >
            <div class="timeline-badge-year">{{ item.year }}</div>
            <div class="timeline-content-card">
              <h3 class="timeline-item-title">{{ item.title }}</h3>
              <p class="timeline-item-desc">{{ item.desc }}</p>
              <span class="timeline-item-tag">{{ item.tag }}</span>
            </div>
          </div>
        </div>
      </section>

      <!-- 2. 主流大模型地图面板 -->
      <section v-if="activeTab === 'landscape'" class="glass-card landscape-panel">
        <h2 class="panel-section-title">主流 AI 厂商与模型格局</h2>
        <p class="panel-section-desc">为您梳理全球与国内最具代表性的 AI 科技巨头、初创独角兽及其基座模型矩阵。</p>
        
        <div class="landscape-grid">
          <div 
            v-for="(vendor, index) in landscapeData" 
            :key="index"
            class="vendor-card"
          >
            <div class="vendor-card-header">
              <span class="vendor-badge" :class="vendor.region === 'global' ? 'global' : 'domestic'">
                {{ vendor.region === 'global' ? '全球先驱' : '国内主力' }}
              </span>
              <h3>{{ vendor.name }}</h3>
            </div>
            <p class="vendor-desc">{{ vendor.description }}</p>
            <div class="vendor-models">
              <strong>旗舰大模型：</strong>
              <div class="model-tag-group">
                <span v-for="(model, mIdx) in vendor.models" :key="mIdx" class="model-tag">
                  {{ model }}
                </span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- 3. 核心工具推荐面板 -->
      <section v-if="activeTab === 'tools'" class="glass-card tools-panel">
        <h2 class="panel-section-title">优秀 AIGC 生产力工具选型</h2>
        <p class="panel-section-desc">聚焦文本生成、图像生成、全栈编程、音频视频等垂直领域，为您挑选当下最实用的 AI 工具。</p>
        
        <!-- 工具分类过滤器 -->
        <div class="tool-filters">
          <button 
            v-for="cat in toolCategories" 
            :key="cat.value"
            class="filter-chip"
            :class="{ active: selectedToolCat === cat.value }"
            @click="selectedToolCat = cat.value"
          >
            {{ cat.label }}
          </button>
        </div>

        <!-- 工具列表 -->
        <div class="tools-grid">
          <div 
            v-for="(tool, index) in filteredTools" 
            :key="index"
            class="tool-card"
          >
            <div class="tool-card-header">
              <h3>{{ tool.name }}</h3>
              <span class="tool-rating">★ {{ tool.rating }}</span>
            </div>
            <p class="tool-desc">{{ tool.desc }}</p>
            <div class="tool-tags">
              <span v-for="(tag, tIdx) in tool.tags" :key="tIdx" class="tool-tag">
                {{ tag }}
              </span>
            </div>
            <div class="tool-card-action">
              <button class="action-btn-sm" @click="handleCopyLink(tool.name)">
                复制工具名称
              </button>
            </div>
          </div>
        </div>
      </section>

      <!-- 4. 术语百科字典面板 -->
      <section v-if="activeTab === 'glossary'" class="glass-card glossary-panel">
        <h2 class="panel-section-title">核心 AI 术语百科</h2>
        <p class="panel-section-desc">为您解答大模型（LLM）、提示工程（Prompt）、检索增强（RAG）等行业黑话与底层技术概念。</p>
        
        <!-- 术语搜索栏 -->
        <div class="search-bar-wrapper">
          <input 
            v-model="searchQuery" 
            type="text" 
            placeholder="搜索 AI 术语、技术指标或缩写..."
            class="glossary-search-input"
          />
        </div>

        <!-- 术语折叠卡片列表 -->
        <div class="glossary-list">
          <div 
            v-for="(item, index) in filteredGlossary" 
            :key="index"
            class="glossary-item-card"
            :class="{ expanded: expandedGlossaryIndex === index }"
            @click="toggleGlossary(index)"
          >
            <div class="glossary-item-header">
              <div class="glossary-term-group">
                <span class="term-abbr">{{ item.term }}</span>
                <span class="term-full-name">{{ item.fullName }}</span>
              </div>
              <span class="expand-indicator">{{ expandedGlossaryIndex === index ? '▲' : '▼' }}</span>
            </div>
            <div v-if="expandedGlossaryIndex === index" class="glossary-item-body">
              <p>{{ item.definition }}</p>
              <div v-if="item.example" class="glossary-example-box">
                <strong>通俗示例：</strong> {{ item.example }}
              </div>
            </div>
          </div>
          <div v-if="filteredGlossary.length === 0" class="no-results-box">
            未找到与 “{{ searchQuery }}” 相关的学术术语。
          </div>
        </div>
      </section>

    </main>

    <!-- 底部隐私与服务条款链接 -->
    <footer class="footer-links">
      <button class="footer-link-btn" @click="showPrivacy = true">Privacy Policy</button>
      <button class="footer-link-btn" @click="showTerms = true">Terms of Service</button>
      <button class="footer-link-btn" @click="showContact = true">Contact Us</button>
      <a href="https://api.wuxian.xyz/sign-up?aff=OyRY" target="_blank" rel="noopener noreferrer" class="footer-link-btn">API 平台</a>
    </footer>

    <!-- 隐私政策弹窗 -->
    <div v-if="showPrivacy" class="modal-overlay" @click.self="showPrivacy = false">
      <div class="modal-content">
        <h3>Privacy Policy</h3>
        <div class="modal-text-content modal-scroll-area">
          <p>我们非常重视您的隐私。作为纯静态科普系统，本页面不涉及任何账户登录，不收集、不上传任何个人偏好或检索记录。</p>
          <p>仅会使用浏览器的本地存储（LocalStorage）记录您的页签浏览偏好，以提供一致的使用体验。</p>
        </div>
        <button class="modal-btn" @click="showPrivacy = false">关闭</button>
      </div>
    </div>

    <!-- 服务条款弹窗 -->
    <div v-if="showTerms" class="modal-overlay" @click.self="showTerms = false">
      <div class="modal-content">
        <h3>Terms of Service</h3>
        <div class="modal-text-content modal-scroll-area">
          <p>欢迎浏览 AI 认知地图百科系统。本应用包含的所有数据与评测分值均基于当前开源技术客观汇编整理，仅供科普展示与工具选型参考。</p>
          <p>本站中引用的所有第三方工具商标、品牌名称与产品权益，归各自厂商所有，本站仅进行客观学术归类。</p>
        </div>
        <button class="modal-btn" @click="showTerms = false">关闭</button>
      </div>
    </div>

    <!-- 联系我们弹窗 -->
    <div v-if="showContact" class="modal-overlay" @click.self="showContact = false">
      <div class="modal-content contact-modal-content">
        <h3>Contact Us</h3>
        <div class="modal-text-content contact-card-body">
          <p>如果您在使用过程中遇到任何问题，或有合作意向，可以通过以下方式联系我们：</p>
          <div class="contact-qr-container">
            <div class="contact-qr-card">
              <img :src="weixinImg" alt="微信交流" class="contact-qr-img" />
              <span class="contact-qr-label">微信交流</span>
            </div>
            <div class="contact-qr-card">
              <img :src="dingtalkImg" alt="钉钉联系" class="contact-qr-img" />
              <span class="contact-qr-label">钉钉联系</span>
            </div>
          </div>
          <p class="contact-email">反馈邮箱: <span style="color: var(--primary-color);">us@wuxian.xyz</span></p>
        </div>
        <button class="modal-btn" @click="showContact = false">关闭</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import weixinImg from '../asset/weixin.png';
import dingtalkImg from '../asset/dingtalk.png';

// 静态站点元信息
const appTitle = ref('AI 认知地图');
const activeTab = ref('timeline');
const selectedToolCat = ref('all');
const searchQuery = ref('');
const expandedGlossaryIndex = ref<number | null>(null);

const copied = ref(false);
const showPrivacy = ref(false);
const showTerms = ref(false);
const showContact = ref(false);

// 1. AI 历史里程碑数据
const timelineData = [
  { year: '1950', title: '艾伦·图灵提出图灵测试', desc: '在论文《计算机器与智能》中定义了判断机器是否具有智能的思想实验，奠定了人工智能的概念根基。', tag: '哲学起源' },
  { year: '1997', title: '深蓝（Deep Blue）击败国际象棋世界冠军', desc: 'IBM 超级计算机击败卡斯帕罗夫，展示了符号计算与强算力搜索在复杂博弈中的惊人能力。', tag: '重大突破' },
  { year: '2012', title: 'AlexNet 夺冠 ImageNet', desc: '在计算机视觉识别大赛中以压倒性优势领先第二名，彻底拉开了深度学习与神经网络革命的序幕。', tag: '深度学习' },
  { year: '2017', title: 'Transformer 架构问世', desc: 'Google 发表论文《Attention Is All You Need》，引入自注意力机制，成为现代所有大语言模型（LLM）的基座底层。', tag: 'NLP革命' },
  { year: '2020', title: 'GPT-3 发布', desc: 'OpenAI 发布 1750 亿参数大模型，首次向世人展示了大模型的涌现能力与强大的通用零样本学习能力。', tag: '大模型时代' },
  { year: '2022', title: 'ChatGPT 开启全民 AI 浪潮', desc: '基于人类反馈强化学习（RLHF）的对话式 AI 爆火，宣告生成式人工智能（AIGC）从学术步入工业级生产力应用。', tag: '全民普及' },
  { year: '2024 - 至今', title: '多模态融合与推理大模型爆发', desc: '文本、画作、视频、音频的深度无缝打通。以 o1 系列及国产开源大模型为代表，AI 迈入深度思考、逻辑推理与智能体爆发时代。', tag: '推理智能体' }
];

// 2. AI 厂商及格局数据
const landscapeData = [
  { name: 'OpenAI', region: 'global', description: '生成式 AI 与大语言模型领域的风向标，开创了 ChatGPT 时代，目前主攻具有自主深度推理能力的 o1 系列模型。', models: ['GPT-4o', 'GPT-4', 'o1-pro', 'DALL-E 3'] },
  { name: 'Anthropic', region: 'global', description: '由 OpenAI 早期核心团队出走创办的明星独角兽，以“宪法 AI”主打对齐与安全，其 Claude 3.5 系列长文本处理与代码生成能力极佳。', models: ['Claude 3.5 Sonnet', 'Claude 3.5 Haiku', 'Claude 3 Opus'] },
  { name: 'Google DeepMind', region: 'global', description: '人工智能科学研究的先驱（AlphaGo 缔造者），Gemini 1.5 具有超长原生上下文和极其强大的全模态联合理解能力。', models: ['Gemini 1.5 Pro', 'Gemini 1.5 Flash', 'Imagen 3'] },
  { name: '阿里通义千问', region: 'domestic', description: '国内开源生态与多模态的领跑者，通义千问（Qwen）开源模型矩阵在多项国际基准评测中名列前茅，中文生态繁荣。', models: ['Qwen 2.5', 'Qwen 2.5-Coder', 'Qwen-VL', 'Qwen-Audio'] },
  { name: 'DeepSeek (深度求索)', region: 'domestic', description: '国产大模型界的一匹超级黑马，以极致的技术效率与性价比横空出世，其推理、数学及代码补全效率处于行业一流水平。', models: ['DeepSeek-V3', 'DeepSeek-Coder', 'DeepSeek-Math'] },
  { name: '智谱 AI', region: 'domestic', description: '清华背景 of 国内大模型独角兽先驱，GLM 系列致力于打造端侧与云端融合的智能体生态，在中英双语认知上表现优异。', models: ['GLM-4-Plus', 'GLM-4-Air', 'CogVideoX'] },
  { name: '月之暗面 (Moonshot)', region: 'domestic', description: '凭借“Kimi 智能助手”掀起超长上下文竞赛的明星企业，支持百万字级别长文档无损解析与多轮跨文档检索交互。', models: ['Kimi Chat', 'Moonshot-v1-200k'] }
];

// 3. 核心工具数据
const toolsData = [
  { name: 'ChatGPT', rating: '4.9', desc: '面向个人与团队的通用智能助手，在问答、文档处理、数据分析和多语言翻译中展现极其稳定的全能表现。', tags: ['通用LLM', 'GPT-4o', '数据分析'], category: 'llm' },
  { name: 'Claude', rating: '4.8', desc: '拥有顶级长文本理解及卓越代码编写能力的 AI。对于程序重构、文档深度提炼和学术研究对齐表现极好。', tags: ['代码分析', '长文本', '高级对齐'], category: 'llm' },
  { name: 'Midjourney', rating: '4.8', desc: '目前生成画质与艺术氛围最强的 AI 绘画平台，擅长概念艺术插图设计、写实照片生成及 UI 原型材质图。', tags: ['图像生成', '原画设计', '艺术创作'], category: 'image' },
  { name: 'v0.dev', rating: '4.9', desc: 'Vercel 打造的 UI 原型与前端代码交互生成平台，通过描述直接输出规范 of React, Vue 与 Tailwind 组件。', tags: ['UI生成', '前端代码', '智能体'], category: 'code' },
  { name: 'Cursor', rating: '4.9', desc: '由 AI 驱动的原生代码编辑器，支持整库上下文理解、多行智能自动补全以及全库级别的全局重构对话。', tags: ['IDE', '代码重构', '高效率'], category: 'code' },
  { name: 'Suno AI', rating: '4.7', desc: '现象级音乐生成平台，支持用户输入歌词与曲风，几秒钟内生成带人声起伏、和声伴奏的完整商业歌曲。', tags: ['音频生成', '词曲创作', '编曲合成'], category: 'audio' },
  { name: 'Runway Gen-3', rating: '4.6', desc: '业内领先的高清视频生成系统，支持从文案/图片直接生成极具电影质感的短视频，镜头发散与动态融合极为平滑。', tags: ['视频生成', '电影运镜', '动态合成'], category: 'video' },
  { name: 'v0 (UI)', rating: '4.8', desc: '全自动组件设计器，支持快速拖曳与主题修改，极大解放了前端的布局还原与骨架生成工作。', tags: ['UI设计', '原型设计'], category: 'image' }
];

const toolCategories = [
  { label: '全部工具', value: 'all' },
  { label: '通用大模型', value: 'llm' },
  { label: '图像及UI生成', value: 'image' },
  { label: '编程辅助', value: 'code' },
  { label: '音频及视频', value: 'audio' }
];

const filteredTools = computed(() => {
  if (selectedToolCat.value === 'all') return toolsData;
  return toolsData.filter(t => t.category === selectedToolCat.value || (selectedToolCat.value === 'audio' && t.category === 'video'));
});

// 4. 百科术语数据
const glossaryData = [
  { term: 'LLM', fullName: 'Large Language Model (大语言模型)', definition: '利用数千亿词元文本进行无监督预训练后，能够理解、交互、推理并生成符合人类逻辑语言的深层神经网络。', example: 'ChatGPT 背后所依托的 GPT-4、千问大模型背后的 Qwen-2.5 都是大语言模型的典型代表。' },
  { term: 'Prompt', fullName: '提示词', definition: '向人工智能大模型输入的文本指令、提示信息或上下文。它是指引、约束与激励大模型输出特定理想结果的核心编程入口。', example: '“帮我写一封语气客气但态度坚决的辞职信，要求列出3个借口” 就是一段典型的 Prompt。' },
  { term: 'RAG', fullName: 'Retrieval-Augmented Generation (检索增强生成)', definition: '由于大模型训练数据存在截止时间且容易产生幻觉，RAG 通过在回答前先检索外部可靠知识库，将检索出来的内容喂给大模型作为背景事实，从而输出准确、实时、贴近实际的应答。', example: '企业级 AI 客服通过读取公司产品说明书 PDF，在不重新训练大模型的情况下，准确回答用户的具体参数问题。' },
  { term: 'Fine-tuning', fullName: '微调', definition: '基于通用的基座模型，使用特定行业、岗位或风格的高质量数据集进行二次训练，使其在保留通用认知能力的基础上，深度适配垂直专业任务的技术。', example: '拿一个通用的医疗大模型，输入十万例儿童感冒诊断案例进行二次训练，使其专门胜任儿科智能导诊任务。' },
  { term: 'Token', fullName: '词元', definition: '大模型进行文本切片、编码与输出概率计算的基本语义单元。中文字符一般 1 个字对应 1~1.5 个 Token；英文约 3/4 个单词为一个 Token。', example: '大模型 API 通常按照 Token 计费，输入和生成的总字符越多，消耗 of Token 也就越多。' },
  { term: 'RLHF', fullName: 'Reinforcement Learning from Human Feedback (人类反馈强化学习)', definition: '通过收集人类评测员对不同回答的偏好评价，训练一个奖励模型，以此引导并微调基座模型，使其输出不仅符合事实，还更对齐人类的道德规范、沟通风格与安全边界。', example: 'ChatGPT 之所以说话客气且不提供制造武器等非法内容，正是RLHF对齐训练所带来的结果。' }
];

const filteredGlossary = computed(() => {
  if (!searchQuery.value.trim()) return glossaryData;
  const q = searchQuery.value.toLowerCase();
  return glossaryData.filter(item => 
    item.term.toLowerCase().includes(q) || 
    item.fullName.toLowerCase().includes(q) || 
    item.definition.toLowerCase().includes(q)
  );
});

const toggleGlossary = (index: number) => {
  if (expandedGlossaryIndex.value === index) {
    expandedGlossaryIndex.value = null;
  } else {
    expandedGlossaryIndex.value = index;
  }
};

const handleCopyLink = async (toolName: string) => {
  try {
    await navigator.clipboard.writeText(toolName);
    copied.value = true;
    setTimeout(() => {
      copied.value = false;
    }, 1500);
  } catch (err) {
    console.error('复制失败', err);
  }
};
</script>

<style scoped>
/* 纯静态百科特定局部样式 */
.static-baike-container {
  max-width: 960px !important;
}

.navigation-tabs {
  margin: 0.5rem 0 1.5rem 0;
}

.main-content-panel {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.panel-section-title {
  font-size: 1.4rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
  color: var(--text-primary);
}

.panel-section-desc {
  color: var(--text-secondary);
  font-size: 0.88rem;
  margin-bottom: 1.5rem;
  line-height: 1.6;
}

/* 1. 时间轴样式 */
.vertical-timeline {
  position: relative;
  padding-left: 2rem;
  border-left: 2px solid rgba(255, 255, 255, 0.08);
  margin-left: 0.5rem;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.timeline-item {
  position: relative;
}

.timeline-badge-year {
  position: absolute;
  left: calc(-2rem - 6px);
  top: 0.25rem;
  background: var(--primary-gradient);
  color: #fff;
  font-size: 0.75rem;
  font-weight: 800;
  padding: 2px 8px;
  border-radius: 99px;
  box-shadow: 0 4px 12px rgba(99, 102, 241, 0.25);
  transform: translateX(-50%);
}

.timeline-content-card {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.04);
  border-radius: 12px;
  padding: 1rem 1.25rem;
  transition: all 0.2s ease;
}

.timeline-content-card:hover {
  background: rgba(255, 255, 255, 0.04);
  border-color: rgba(99, 102, 241, 0.2);
  transform: translateX(4px);
}

.timeline-item-title {
  font-size: 1.05rem;
  font-weight: 600;
  margin-bottom: 0.4rem;
  color: var(--text-primary);
}

.timeline-item-desc {
  font-size: 0.88rem;
  color: var(--text-secondary);
  line-height: 1.6;
  margin-bottom: 0.6rem;
}

.timeline-item-tag {
  display: inline-block;
  font-size: 0.72rem;
  background: rgba(255, 255, 255, 0.06);
  padding: 2px 8px;
  border-radius: 4px;
  color: var(--text-secondary);
}

/* 2. 大模型格局网格 */
.landscape-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

@media (min-width: 768px) {
  .landscape-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

.vendor-card {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.04);
  border-radius: 12px;
  padding: 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.vendor-card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.vendor-card-header h3 {
  font-size: 1.15rem;
  font-weight: 700;
  color: var(--text-primary);
}

.vendor-badge {
  font-size: 0.7rem;
  font-weight: 600;
  padding: 2px 8px;
  border-radius: 99px;
  border: 1px solid transparent;
}

.vendor-badge.global {
  background: rgba(16, 185, 129, 0.1);
  color: #10b981;
  border-color: rgba(16, 185, 129, 0.2);
}

.vendor-badge.domestic {
  background: rgba(99, 102, 241, 0.1);
  color: #6366f1;
  border-color: rgba(99, 102, 241, 0.2);
}

.vendor-desc {
  font-size: 0.85rem;
  color: var(--text-secondary);
  line-height: 1.5;
  flex-grow: 1;
}

.vendor-models {
  font-size: 0.8rem;
  border-top: 1px solid rgba(255, 255, 255, 0.04);
  padding-top: 0.6rem;
}

.model-tag-group {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
  margin-top: 0.4rem;
}

.model-tag {
  font-size: 0.75rem;
  background: rgba(255, 255, 255, 0.05);
  color: var(--text-primary);
  padding: 2px 6px;
  border-radius: 4px;
  border: 1px solid rgba(255, 255, 255, 0.05);
}

/* 3. 工具选型样式 */
.tool-filters {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 1.25rem;
}

.filter-chip {
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.04);
  color: var(--text-secondary);
  padding: 0.4rem 0.8rem;
  border-radius: 99px;
  font-size: 0.8rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}

.filter-chip:hover {
  background: rgba(255, 255, 255, 0.08);
  color: var(--text-primary);
}

.filter-chip.active {
  background: var(--primary-gradient);
  color: #fff;
  border-color: transparent;
  box-shadow: 0 4px 12px rgba(99, 102, 241, 0.25);
}

.tools-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

@media (min-width: 768px) {
  .tools-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

.tool-card {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.04);
  border-radius: 12px;
  padding: 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  transition: all 0.2s ease;
}

.tool-card:hover {
  background: rgba(255, 255, 255, 0.04);
  border-color: rgba(99, 102, 241, 0.15);
  transform: translateY(-2px);
}

.tool-card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.tool-card-header h3 {
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--text-primary);
}

.tool-rating {
  font-size: 0.8rem;
  color: #fbbf24;
  font-weight: 700;
}

.tool-desc {
  font-size: 0.85rem;
  color: var(--text-secondary);
  line-height: 1.5;
  flex-grow: 1;
}

.tool-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
}

.tool-tag {
  font-size: 0.72rem;
  background: rgba(99, 102, 241, 0.06);
  color: #a5b4fc;
  padding: 2px 6px;
  border-radius: 4px;
}

.tool-card-action {
  border-top: 1px solid rgba(255, 255, 255, 0.04);
  padding-top: 0.6rem;
  text-align: right;
}

.action-btn-sm {
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(255, 255, 255, 0.06);
  color: var(--text-primary);
  font-size: 0.75rem;
  padding: 4px 10px;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.action-btn-sm:hover {
  background: var(--primary-gradient);
  color: #fff;
  border-color: transparent;
}

/* 4. 术语百科样式 */
.search-bar-wrapper {
  margin-bottom: 1.25rem;
}

.glossary-search-input {
  width: 100%;
  background: rgba(0, 0, 0, 0.25);
  border: 1px solid var(--card-border);
  border-radius: 12px;
  padding: 0.75rem 1rem;
  color: var(--text-primary);
  font-size: 0.92rem;
  font-family: inherit;
  outline: none;
  transition: all 0.2s ease;
}

.glossary-search-input:focus {
  border-color: #6366f1;
  box-shadow: 0 0 0 2px rgba(99, 102, 241, 0.2);
}

.glossary-list {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.glossary-item-card {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.04);
  border-radius: 10px;
  padding: 0.85rem 1.2rem;
  cursor: pointer;
  transition: all 0.25s ease;
}

.glossary-item-card:hover {
  background: rgba(255, 255, 255, 0.04);
  border-color: rgba(255, 255, 255, 0.08);
}

.glossary-item-card.expanded {
  background: rgba(255, 255, 255, 0.03);
  border-color: rgba(99, 102, 241, 0.15);
}

.glossary-item-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.glossary-term-group {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.term-abbr {
  font-size: 1.05rem;
  font-weight: 700;
  color: #a855f7;
}

.term-full-name {
  font-size: 0.85rem;
  color: var(--text-secondary);
}

.expand-indicator {
  font-size: 0.75rem;
  color: var(--text-secondary);
}

.glossary-item-body {
  margin-top: 0.75rem;
  padding-top: 0.75rem;
  border-top: 1px solid rgba(255, 255, 255, 0.04);
  font-size: 0.88rem;
  color: var(--text-secondary);
  line-height: 1.6;
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
}

.glossary-example-box {
  background: rgba(0, 0, 0, 0.15);
  border-radius: 6px;
  padding: 0.5rem 0.75rem;
  font-size: 0.82rem;
  color: var(--text-primary);
  border-left: 3px solid #10b981;
}

.no-results-box {
  text-align: center;
  padding: 2rem;
  color: var(--text-secondary);
  font-size: 0.88rem;
}
</style>
