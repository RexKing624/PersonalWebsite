<script setup>
import { computed, onMounted, ref, watch } from 'vue'
import PhotographyPage from './PhotographyPage.vue'

const translations = {
  zh: {
    language: '语言选择', theme: '切换深浅主题', home: '回到首页', nav: ['作品', '介绍', '联系'], hello: '你好，我是雷酱',
    seeWork: '看看我的作品',
    featured: '精选项目', photography: '摄影作品',
    roles: ['攻读 PhD 中', 'HCI 研究者', '全栈开发工程师', '胶片摄影爱好者', '爵士吉他手'],
    codeLabel: 'Skills', slogan: 'Code is only my tool, Emotion is what I create.',
    contactLabel: '联系我', source: '在 GitHub 仓库中查看', location: '东京 · 日本',
    projects: [
      ['XerFilmlab', 'Film Photography & Archive Platform', '以胶片摄影为核心的个人厂牌与数字化整理平台。除了展示胶片、人像、街头摄影和宝丽来作品，也用于管理胶卷、相册、拍摄信息、器材与扫描文件，让拍摄、冲洗、扫描、归档和展示形成完整的胶片工作流。', 'Film Photography · Film Archive · Gallery · Analog Workflow', '研究需要，展示不公开', ''],
      ['六爻纳甲起卦', 'A Modern I Ching Divination Experience', '一款将传统《周易》起卦过程转化为现代交互体验的应用。用户提出问题后，通过三枚硬币完成纳甲筮法中的铜钱起卦，生成六爻卦象并解读本卦、变爻与变卦。它既可用于日常占问，也是一种带有仪式感的随机生成与符号解码工具，可服务于谜题、叙事、创作灵感与信息解密。', 'I Ching · Six Lines · Divination · Interaction · Symbolic Decoding', 'App Store 尚未发布', ''],
      ['EchoSpell', 'English Listening and Spelling Practice', '一款面向中文和日文用户的英语听音拼写练习工具，专门解决“会念、听得懂，但不会写”。用户可以使用系统单词表，也可以上传自己的单词表、文章或学习资料。系统提取词汇并播放发音，学习模式支持对照抄写，练习模式只播放读音并由用户完成拼写，同时结合释义、重复听音、错误记录与复习机制。', 'Listening · Spelling · English Learning · Chinese & Japanese Learners', '访问 EchoSpell', 'http://echospell.xergnik.com'],
      ['emotionChat', 'Local AI Persona Distillation Environment', '一个基于 Ollama、Express 与 Vue 3 搭建的本地 AI 聊天环境，用于从指定资料中蒸馏一个人的语言习惯、记忆、经历与表达方式。用户导入聊天记录、文字资料、人物背景和关系信息后，系统在本地构建对应的人格角色。项目关注人格如何由记忆与表达构成、AI 如何承载关系与情感投射，以及本地模型如何保护高度私密的个人资料。', 'Local AI · Persona Distillation · Memory · Digital Presence · Ollama', '查看 GitHub', 'https://github.com/RexKing624/emotionChat'],
    ],
  },
  ja: {
    language: '言語を選択', theme: 'テーマを切り替える', home: 'ホームへ戻る', nav: ['作品', '自己紹介', '連絡先'], hello: 'こんにちは、レです',
    seeWork: '作品を見る',
    featured: '注目のプロジェクト', photography: '写真作品',
    roles: ['PhD取得に向けて勉強中', 'HCI研究者', 'フルスタックエンジニア', 'フィルム写真愛好家', 'ジャズギタリスト'],
    codeLabel: 'Skills', slogan: 'Code is only my tool, Emotion is what I create.',
    contactLabel: '連絡先', source: 'GitHubリポジトリで見る', location: '東京 · 日本',
    projects: [
      ['XerFilmlab', 'Film Photography & Archive Platform', 'フィルム写真を軸にした個人ブランド兼デジタルアーカイブ基盤です。フィルム、ポートレート、ストリート、ポラロイド作品を展示するだけでなく、ロール、アルバム、撮影情報、機材、スキャンデータを管理し、撮影から現像、スキャン、整理、公開までを一つのワークフローとしてつなぎます。', 'Film Photography · Film Archive · Gallery · Analog Workflow', '研究上の理由により非公開', ''],
      ['六爻見', 'A Modern I Ching Divination Experience', '伝統的な『易経』の卦立てを、現代的なインタラクションへ変換するアプリです。問いを決めた後、三枚の硬貨で六爻の卦を立て、本卦・変爻・之卦を読み解きます。日常の占いに加え、儀式性を備えたランダム生成と記号解読の道具として、謎解き、物語、創作、情報の暗号化にも活用できます。', 'I Ching · Six Lines · Divination · Interaction · Symbolic Decoding', 'App Store 未公開', ''],
      ['EchoSpell', 'English Listening and Spelling Practice', '中国語・日本語話者向けの英語リスニング／スペリング練習ツールです。「発音でき、聞き取れるのに書けない」という課題に特化しています。内蔵単語帳に加え、単語リストや文章、学習資料をアップロード可能。学習モードでは見本を見ながら書き、練習モードでは音声だけを聞いて入力します。意味表示、再生、誤答記録、復習も組み合わせます。', 'Listening · Spelling · English Learning · Chinese & Japanese Learners', 'EchoSpellを見る', 'http://echospell.xergnik.com'],
      ['emotionChat', 'Local AI Persona Distillation Environment', 'Ollama、Express、Vue 3で構築するローカルAIチャット環境です。会話ログや文章、人物背景、関係性の情報から、その人の言語習慣、記憶、経験、表現を抽出し、ローカル環境に人格を再構成します。人格を形づくる記憶と表現、AIが担う関係や感情の投影、そして機密性の高い個人データをローカルモデルで守る方法を探ります。', 'Local AI · Persona Distillation · Memory · Digital Presence · Ollama', 'GitHubを見る', 'https://github.com/RexKing624/emotionChat'],
    ],
  },
  en: {
    language: 'Choose language', theme: 'Toggle color theme', home: 'Back to home', nav: ['Work', 'About', 'Contact'], hello: "Hi, I'm Rex",
    seeWork: 'See my work',
    featured: 'Selected projects', photography: 'Photography',
    roles: ['Pursuing a PhD', 'HCI Researcher', 'Full-stack Developer', 'Film Photography Enthusiast', 'Jazz Guitarist'],
    codeLabel: 'Skills', slogan: 'Code is only my tool, Emotion is what I create.',
    contactLabel: 'Contact', source: 'View on GitHub', location: 'Tokyo · Japan',
    projects: [
      ['XerFilmlab', 'Film Photography & Archive Platform', 'A personal film photography label and digital archive platform. Beyond presenting film, portrait, street, and Polaroid work, it manages rolls, albums, shooting metadata, equipment, and scans—connecting shooting, developing, scanning, archiving, and publishing into one complete analog workflow.', 'Film Photography · Film Archive · Gallery · Analog Workflow', 'Private for research purposes', ''],
      ['LineSight', 'A Modern I Ching Divination Experience', 'An application that transforms the traditional I Ching divination process into a modern interactive experience. Users frame a question, cast three coins to form a six-line hexagram, and interpret the original hexagram, changing lines, and resulting hexagram. It also works as a ritualized system for random generation and symbolic decoding in puzzles, narratives, creative prompts, and information ciphers.', 'I Ching · Six Lines · Divination · Interaction · Symbolic Decoding', 'Not yet available on the App Store', ''],
      ['EchoSpell', 'English Listening and Spelling Practice', 'A listening and spelling tool for Chinese- and Japanese-speaking English learners, built around a common gap: you can pronounce and understand a word, but cannot spell it. Learners can use built-in lists or upload vocabulary, articles, and study materials. Study mode supports guided copying; practice mode plays audio only and asks learners to type, with definitions, replay, error tracking, and review.', 'Listening · Spelling · English Learning · Chinese & Japanese Learners', 'Visit EchoSpell', 'http://echospell.xergnik.com'],
      ['emotionChat', 'Local AI Persona Distillation Environment', 'A local AI chat environment built with Ollama, Express, and Vue 3. It distills a person’s speech patterns, memories, experiences, and expression from conversations, writing, background, and relationship context, then reconstructs that persona locally. The project explores what makes a personality stable, how AI carries memory and emotional projection, and how local models protect deeply private material.', 'Local AI · Persona Distillation · Memory · Digital Presence · Ollama', 'View on GitHub', 'https://github.com/RexKing624/emotionChat'],
    ],
  },
}

const savedLanguage = localStorage.getItem('site-language')
const browserLanguage = typeof navigator === 'undefined' ? '' : (navigator.language || '')
const detectedLanguage = browserLanguage.toLowerCase().startsWith('zh')
  ? 'zh'
  : browserLanguage.toLowerCase().startsWith('ja') ? 'ja' : 'en'
const language = ref(['zh', 'ja', 'en'].includes(savedLanguage) ? savedLanguage : detectedLanguage)
const savedTheme = localStorage.getItem('site-theme')
const theme = ref(savedTheme === 'light' ? 'light' : 'dark')
const t = computed(() => translations[language.value])
const imageSets = computed(() => [
  [`/assets/projects/xer-${language.value}.png`],
  ['/assets/projects/linesight-1.png', '/assets/projects/linesight-2.png', '/assets/projects/linesight-3.png'],
  ['/assets/projects/echospell.png'],
  ['/assets/projects/emotionchat.png'],
])
const projects = computed(() => t.value.projects.map((project, index) => ({
  title: project[0] === 'XerFilmlab' ? 'XerFilmLab' : project[0] === 'emotionChat' ? 'EmotionChat' : project[0], subtitle: project[1], description: project[2], tags: project[3], action: project[4], url: project[5], images: imageSets.value[index],
})))

watch(language, (value) => {
  localStorage.setItem('site-language', value)
  document.documentElement.lang = value === 'zh' ? 'zh-CN' : value === 'ja' ? 'ja' : 'en'
}, { immediate: true })

watch(theme, (value) => {
  localStorage.setItem('site-theme', value)
  document.documentElement.dataset.theme = value
}, { immediate: true })

const scrollToWork = () => document.querySelector('#work')?.scrollIntoView({ behavior: 'smooth' })
const isPhotographyPage = window.location.pathname.replace(/\/$/, '') === '/photography'

onMounted(() => {
  const items = document.querySelectorAll('.reveal-project')
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    items.forEach((item) => item.classList.add('is-visible'))
    return
  }

  const observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (!entry.isIntersecting) return
      entry.target.classList.add('is-visible')
      observer.unobserve(entry.target)
    })
  }, { threshold: 0.12, rootMargin: '0px 0px -6% 0px' })

  items.forEach((item) => observer.observe(item))
})
</script>

<template>
  <PhotographyPage v-if="isPhotographyPage" />
  <div v-else class="page-shell">
    <header class="nav">
      <a class="mark" href="#top" :aria-label="t.home"><img :src="theme === 'light' ? '/assets/logo-w.png' : '/assets/logo-b.png'" alt="Rex" /></a>
      <div class="nav-right">
        <nav aria-label="主导航">
          <a href="#work">{{ t.nav[0] }}</a>
          <a href="#about">{{ t.nav[1] }}</a>
          <a href="mailto:hello@example.com">{{ t.nav[2] }}</a>
        </nav>
        <div class="language-switcher" role="group" :aria-label="t.language">
          <button v-for="item in [{ id: 'en', label: 'EN' }, { id: 'ja', label: '日' }, { id: 'zh', label: '中' }]" :key="item.id" type="button" :class="{ active: language === item.id }" :aria-pressed="language === item.id" @click="language = item.id">{{ item.label }}</button>
        </div>
        <button class="theme-switcher" type="button" :aria-label="t.theme" @click="theme = theme === 'light' ? 'dark' : 'light'">
          <span class="theme-track" aria-hidden="true"><span :class="{ dark: theme === 'dark' }"></span></span>
        </button>
      </div>
    </header>

    <main id="top">
      <section id="about" class="about" aria-labelledby="about-title">
        <h2 id="about-title">{{ t.hello }}</h2>
        <div class="about-layout">
          <div class="about-copy">
            <ul class="roles" aria-label="Profile">
              <li v-for="role in t.roles" :key="role">{{ role }}</li>
            </ul>
            <div class="code-list">
              <p>{{ t.codeLabel }}</p>
              <div><span v-for="code in ['C', 'Java', 'JavaScript', 'PHP']" :key="code">{{ code }}</span></div>
            </div>
          </div>
          <figure class="portrait-card">
            <img src="/assets/rex-portrait.png" alt="Rex in Tokyo at night" />
            <figcaption>
              <a href="https://x.com/Xer_Gnik" target="_blank" rel="noreferrer">X <span>@Xer_Gnik</span></a>
              <a href="https://github.com/Rexking624" target="_blank" rel="noreferrer">GitHub <span>Rexking624</span></a>
              <a href="https://www.instagram.com/RexKing624" target="_blank" rel="noreferrer">Instagram <span>@RexKing624</span></a>
            </figcaption>
          </figure>
        </div>
        <div class="about-footer">
          <blockquote>{{ t.slogan }}</blockquote>
          <button type="button" @click="scrollToWork">{{ t.seeWork }} <span aria-hidden="true">↓</span></button>
        </div>
      </section>

      <section id="work" class="work" aria-labelledby="work-title">
        <div class="section-heading">
          <p id="work-title">{{ t.featured }}</p>
          <a class="photography-link" href="/photography">{{ t.photography }} <span>↗</span></a>
        </div>

        <article v-for="(project, index) in projects" :key="project.title" class="project reveal-project" :style="{ '--reveal-delay': `${index * 70}ms` }">
          <div class="project-art">
            <div v-if="project.images.length > 1" class="screenshot-stack">
              <img v-for="(image, imageIndex) in project.images" :key="image" :src="image" :alt="`${project.title} screenshot ${imageIndex + 1}`" />
            </div>
            <img v-else class="project-screenshot" :src="project.images[0]" :alt="`${project.title} screenshot`" />
          </div>
          <div class="project-copy">
            <p>{{ project.subtitle }}</p>
            <h3>{{ project.title }}</h3>
            <p>{{ project.description }}</p>
            <p class="project-tags">{{ project.tags }}</p>
            <a v-if="project.url" class="project-action" :href="project.url" target="_blank" rel="noreferrer">{{ project.action }} <span>↗</span></a>
            <p v-else class="project-status"><span></span>{{ project.action }}</p>
          </div>
        </article>
      </section>

      <section class="contact" aria-labelledby="contact-title">
        <p id="contact-title">{{ t.contactLabel }}</p>
        <a href="mailto:hello@xergnik.com">hello@xergnik.com <span>↗</span></a>
      </section>
    </main>

    <footer>
      <p>© 2026 RexKing624</p>
      <p>{{ t.location }}</p>
      <div><a class="source-placeholder" href="https://github.com/RexKing624/PersonalWebsite" target="_blank" rel="noreferrer">{{ t.source }} <span>↗</span></a></div>
    </footer>
  </div>
</template>
