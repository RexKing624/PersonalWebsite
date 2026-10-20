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
    contactLabel: '联系我', source: 'GitHub', location: '东京 · 日本',
    projects: [
      ['XerFilmLab Web', 'Film Archive Management Web App', '面向胶片摄影工作流的 Web 管理系统，用于整理胶卷、相册、拍摄信息、器材、扫描文件与作品展示，让拍摄、冲洗、扫描、归档和发布形成可检索的数字档案。', 'Web App · Film Archive · Gallery · Analog Workflow', '研究需要，展示不公开', ''],
      ['XerFilmLab for macOS', 'Native Film Negative Conversion Workflow', '一款原生 macOS 胶片负片去色罩与校色应用。从 RAW 和高位深扫描开始，依次完成片基提取、彩色或黑白负片转换、颜色与明暗调整、RGB 校正、除尘、齿孔、几何裁切、胶片元数据和预设保存。', 'macOS · SwiftUI · Negative Conversion · Color Workflow', '', '', [['下载', 'https://lab.xergnik.com'], ['GitHub', 'https://github.com/RexKing624/XerFilmLab-Website']]],
      ["XerFilm GR", "Film Character, Inside Your GR", "适用于 RICOH GR IV / GR IV HDF、GR III / GR III HDF 与 GR IIIx / GR IIIx HDF 的非官方胶片模拟固件。17 个胶片家族、27 种效果，通过胶片列表管理常用风格，在 ADJ 母子菜单中预览和切换，再用与 ISO 联动的颗粒和日期打印完成 JPG 直出。项目把胶片选择、预览与拍摄整理为一个连续的机内交互流程。", "RICOH GR Series · Film Simulation · ADJ · ISO Grain · Vue 3", "", "", [["访问 XerFilm GR", "https://filmgr.xergnik.com/"], ["GitHub", "https://github.com/RexKing624/XerFilm-GR-Website"]]],
      ['ONE ARCANA', 'A Quiet One-Card Tarot Experience', '一个把塔罗抽牌收束为提问、洗牌、选牌与翻牌的极简 Web 实验。它不替用户预测未来，而是以可触摸的手势、克制的声音和完整的 78 张牌组，让数字随机仍保留亲手选择的感觉。抽牌使用浏览器加密随机数，结果只提供简短的传统牌义，并可整理成图片或提示词交给用户自己的 AI Assistant。项目关注数字界面如何保留仪式感、个人判断与最终解释权。', 'Tarot · Gesture Interaction · Web Audio · Multilingual · Cryptographic Randomness', '', '', [['访问 ONE ARCANA', 'https://onearcana.xergnik.com/'], ['GitHub', 'https://github.com/RexKing624/ONEARCANA']]],
      ['六爻纳甲起卦', 'A Modern I Ching Divination Experience', '一款将传统《周易》起卦过程转化为现代交互体验的应用。用户提出问题后，通过三枚硬币完成纳甲筮法中的铜钱起卦，生成六爻卦象并解读本卦、变爻与变卦。它既可用于日常占问，也是一种带有仪式感的随机生成与符号解码工具，可服务于谜题、叙事、创作灵感与信息解密。', 'I Ching · Six Lines · Divination · Interaction · Symbolic Decoding', 'App Store 尚未发布', ''],
      ['EchoSpell', 'English Listening and Spelling Practice', '一款面向中文和日文用户的英语听音拼写练习工具，专门解决“会念、听得懂，但不会写”。用户可以使用系统单词表，也可以上传自己的单词表、文章或学习资料。系统提取词汇并播放发音，学习模式支持对照抄写，练习模式只播放读音并由用户完成拼写，同时结合释义、重复听音、错误记录与复习机制。', 'Listening · Spelling · English Learning · Chinese & Japanese Learners', '', '', [['访问 EchoSpell', 'https://echospell.xergnik.com'], ['GitHub', 'https://github.com/RexKing624/echo-spell']]],
      ["emotionChat", "让回忆融入日常对话", "一个以本地隐私为出发点的 AI 聊天应用，让人物设定与共同回忆融入日常对话。导入整理好的回忆主题，连接本机 Ollama 模型，角色便能参考过往内容，以自己的性格回应或主动开启话题。回忆与对话在本机处理，聊天记录以 Markdown 保存在本地，无需将私密资料发送给云端模型。项目探索人格如何由记忆与表达构成、AI 如何承载关系与情感投射，以及本地运行如何让个人资料的保管与使用掌握在用户手中。", "Local AI · Privacy · Memory · Persona · Digital Presence", "GitHub", "https://github.com/RexKing624/emotionChat"],
    ],
  },
  ja: {
    language: '言語を選択', theme: 'テーマを切り替える', home: 'ホームへ戻る', nav: ['作品', '自己紹介', '連絡先'], hello: 'こんにちは、レです',
    seeWork: '作品を見る',
    featured: '注目のプロジェクト', photography: '写真作品',
    roles: ['PhD取得に向けて勉強中', 'HCI研究者', 'フルスタックエンジニア', 'フィルム写真愛好家', 'ジャズギタリスト'],
    codeLabel: 'Skills', slogan: 'Code is only my tool, Emotion is what I create.',
    contactLabel: '連絡先', source: 'GitHub', location: '東京 · 日本',
    projects: [
      ['XerFilmLab Web', 'Film Archive Management Web App', 'フィルム写真のための Web 管理システムです。ロール、アルバム、撮影情報、機材、スキャンデータ、作品公開を整理し、撮影・現像・スキャン・アーカイブ・公開を検索可能なデジタル記録へまとめます。', 'Web App · Film Archive · Gallery · Analog Workflow', '研究上の理由により非公開', ''],
      ['XerFilmLab for macOS', 'Native Film Negative Conversion Workflow', 'RAW や高ビット深度スキャンから始め、フィルムベース抽出、カラー／白黒ネガ変換、カラーとトーン、RGB 補正、ゴミ取り、パーフォレーション、クロップ、メタデータ、プリセット保存までを扱う macOS ネイティブアプリです。', 'macOS · SwiftUI · Negative Conversion · Color Workflow', '', '', [['ダウンロード', 'https://lab.xergnik.com'], ['GitHub', 'https://github.com/RexKing624/XerFilmLab-Website']]],
      ["XerFilm GR", "Film Character, Inside Your GR", "RICOH GR IV / GR IV HDF、GR III / GR III HDF、GR IIIx / GR IIIx HDF に対応する非公式フィルムシミュレーションです。17 ファミリー、27 ルックをフィルム一覧で管理し、ADJ の親子メニューからプレビュー・切り替え。ISO と連動する粒状感と日付印字を組み合わせ、JPG に仕上げます。フィルムを選ぶ、確認する、撮るという動作を、カメラの中のひと続きの体験として設計しました。", "RICOH GR Series · Film Simulation · ADJ · ISO Grain · Vue 3", "", "", [["XerFilm GR へ", "https://filmgr.xergnik.com/"], ["GitHub", "https://github.com/RexKing624/XerFilm-GR-Website"]]],
      ['ONE ARCANA', 'A Quiet One-Card Tarot Experience', 'タロットを「問い、シャッフル、選択、カードを開く」という最小限の体験に整理した Web 実験です。未来を自動で予言するのではなく、触れるジェスチャー、抑制された音、78 枚のカードによって、デジタルなランダム性の中にも自分で選ぶ感覚を残します。抽選にはブラウザの暗号学的乱数を使い、結果は短い伝統的な意味だけを提示。画像やプロンプトとして自分の AI Assistant に渡せます。デジタル UI が儀式性、判断、解釈の余白をどう保てるかを探るプロジェクトです。', 'Tarot · Gesture Interaction · Web Audio · Multilingual · Cryptographic Randomness', '', '', [['ONE ARCANA へ', 'https://onearcana.xergnik.com/'], ['GitHub', 'https://github.com/RexKing624/ONEARCANA']]],
      ['六爻見', 'A Modern I Ching Divination Experience', '伝統的な『易経』の卦立てを、現代的なインタラクションへ変換するアプリです。問いを決めた後、三枚の硬貨で六爻の卦を立て、本卦・変爻・之卦を読み解きます。日常の占いに加え、儀式性を備えたランダム生成と記号解読の道具として、謎解き、物語、創作、情報の暗号化にも活用できます。', 'I Ching · Six Lines · Divination · Interaction · Symbolic Decoding', 'App Store 未公開', ''],
      ['EchoSpell', 'English Listening and Spelling Practice', '中国語・日本語話者向けの英語リスニング／スペリング練習ツールです。「発音でき、聞き取れるのに書けない」という課題に特化しています。内蔵単語帳に加え、単語リストや文章、学習資料をアップロード可能。学習モードでは見本を見ながら書き、練習モードでは音声だけを聞いて入力します。意味表示、再生、誤答記録、復習も組み合わせます。', 'Listening · Spelling · English Learning · Chinese & Japanese Learners', '', '', [['EchoSpell へ', 'https://echospell.xergnik.com'], ['GitHub', 'https://github.com/RexKing624/echo-spell']]],
      ["emotionChat", "記憶とつながるローカルAIチャット", "プライバシーを大切にするローカルAIチャットアプリです。人物設定と共有の記憶を読み込み、手元のOllamaモデルに接続すると、キャラクターが過去の会話を踏まえ、自分らしい口調で返事をしたり、話しかけたりします。記憶と会話は端末内で処理され、履歴はローカルのMarkdownファイルに保存。プライベートな資料をクラウドのAIモデルへ送る必要はありません。記憶と表現が人格を形づくる過程、AIが関係性や感情の投影を受け止めるあり方、そして個人の資料を自分の手元で管理することの意味を探ります。", "Local AI · Privacy · Memory · Persona · Digital Presence", "GitHub", "https://github.com/RexKing624/emotionChat"],
    ],
  },
  en: {
    language: 'Choose language', theme: 'Toggle color theme', home: 'Back to home', nav: ['Work', 'About', 'Contact'], hello: "Hi, I'm Rex",
    seeWork: 'See my work',
    featured: 'Selected projects', photography: 'Photography',
    roles: ['Pursuing a PhD', 'HCI Researcher', 'Full-stack Developer', 'Film Photography Enthusiast', 'Jazz Guitarist'],
    codeLabel: 'Skills', slogan: 'Code is only my tool, Emotion is what I create.',
    contactLabel: 'Contact', source: 'GitHub', location: 'Tokyo · Japan',
    projects: [
      ['XerFilmLab Web', 'Film Archive Management Web App', 'A web-based management system for film photography workflows. It organizes rolls, albums, shooting details, equipment, scans, and published work—turning shooting, development, scanning, archiving, and presentation into a searchable digital record.', 'Web App · Film Archive · Gallery · Analog Workflow', 'Private for research purposes', ''],
      ['XerFilmLab for macOS', 'Native Film Negative Conversion Workflow', 'A native macOS application for film-base removal and color correction. Starting from RAW or high-bit-depth scans, it covers film-base sampling, color and B&W negative conversion, color, tone and RGB correction, dust healing, sprockets, crop, film metadata, and reusable presets.', 'macOS · SwiftUI · Negative Conversion · Color Workflow', '', '', [['Download', 'https://lab.xergnik.com'], ['GitHub', 'https://github.com/RexKing624/XerFilmLab-Website']]],
      ["XerFilm GR", "Film Character, Inside Your GR", "Unofficial film-simulation firmware for RICOH GR IV / GR IV HDF, GR III / GR III HDF, and GR IIIx / GR IIIx HDF. Organize 17 film families and 27 looks in the Film List, preview and switch through ADJ parent and child menus, then finish JPGs with ISO-aware grain and date stamping. The project brings choosing, previewing, and shooting into one connected in-camera interaction.", "RICOH GR Series · Film Simulation · ADJ · ISO Grain · Vue 3", "", "", [["Visit XerFilm GR", "https://filmgr.xergnik.com/"], ["GitHub", "https://github.com/RexKing624/XerFilm-GR-Website"]]],
      ['ONE ARCANA', 'A Quiet One-Card Tarot Experience', 'A minimal web experiment that reduces tarot to asking, shuffling, choosing, and revealing. It does not try to predict the future for the user. Tactile gestures, restrained sound, and a complete 78-card deck preserve the feeling of making a personal choice inside a digital random process. Draws use browser cryptographic randomness, while results stay concise and can be exported as an image or prompt for the user’s own AI Assistant. The project explores how a digital interface can retain ritual, judgment, and room for interpretation.', 'Tarot · Gesture Interaction · Web Audio · Multilingual · Cryptographic Randomness', '', '', [['Visit ONE ARCANA', 'https://onearcana.xergnik.com/'], ['GitHub', 'https://github.com/RexKing624/ONEARCANA']]],
      ['LineSight', 'A Modern I Ching Divination Experience', 'An application that transforms the traditional I Ching divination process into a modern interactive experience. Users frame a question, cast three coins to form a six-line hexagram, and interpret the original hexagram, changing lines, and resulting hexagram. It also works as a ritualized system for random generation and symbolic decoding in puzzles, narratives, creative prompts, and information ciphers.', 'I Ching · Six Lines · Divination · Interaction · Symbolic Decoding', 'Not yet available on the App Store', ''],
      ['EchoSpell', 'English Listening and Spelling Practice', 'A listening and spelling tool for Chinese- and Japanese-speaking English learners, built around a common gap: you can pronounce and understand a word, but cannot spell it. Learners can use built-in lists or upload vocabulary, articles, and study materials. Study mode supports guided copying; practice mode plays audio only and asks learners to type, with definitions, replay, error tracking, and review.', 'Listening · Spelling · English Learning · Chinese & Japanese Learners', '', '', [['Visit EchoSpell', 'https://echospell.xergnik.com'], ['GitHub', 'https://github.com/RexKing624/echo-spell']]],
      ["emotionChat", "Local AI Conversations with Memory", "A privacy-focused local AI chat app that brings personas and shared memories into everyday conversation. Import a memory theme and connect an Ollama model running on your own device. Characters draw on past conversations to reply and start new topics in their own voice. Memories and conversations are processed on your device, with chat history saved locally as Markdown—without sending private material to a cloud AI model. The project explores how memory and expression shape personality, how AI carries relationships and emotional projection, and how local processing keeps personal material under the user’s control.", "Local AI · Privacy · Memory · Persona · Digital Presence", "GitHub", "https://github.com/RexKing624/emotionChat"],
    ],
  },
}

const browserLanguage = typeof navigator === 'undefined' ? '' : (navigator.language || '')
const detectedLanguage = browserLanguage.toLowerCase().startsWith('zh')
  ? 'zh'
  : browserLanguage.toLowerCase().startsWith('ja') ? 'ja' : 'en'
const language = ref(detectedLanguage)
const savedTheme = localStorage.getItem('site-theme')
const theme = ref(savedTheme === 'light' ? 'light' : 'dark')
const t = computed(() => translations[language.value])
const imageSets = computed(() => [
  [`/assets/projects/xer-${language.value}.png`],
  [`/assets/projects/xerfilmlab-macos-${language.value}.png`],
  [`/assets/projects/xerfilm-gr-${language.value}.svg`],
  [
    `/assets/projects/onearcana-${language.value}-1.png`,
    `/assets/projects/onearcana-${language.value}-2.png`,
    `/assets/projects/onearcana-${language.value}-3-v2.png`,
  ],
  ['/assets/projects/linesight-1.png', '/assets/projects/linesight-2.png', '/assets/projects/linesight-3.png'],
  ['/assets/projects/echospell.png'],
  [`/assets/projects/emotionchat-${language.value}.svg`],
])
const projects = computed(() => t.value.projects.map((project, index) => ({
  title: project[0] === 'XerFilmlab' ? 'XerFilmLab' : project[0] === 'emotionChat' ? 'EmotionChat' : project[0], subtitle: project[1], description: project[2], tags: project[3], action: project[4], url: project[5], links: project[6] || (project[5] ? [[project[4], project[5]]] : []), images: imageSets.value[index],
})))

watch(language, (value) => {
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
          <a href="mailto:hello@xergnik.com">{{ t.nav[2] }}</a>
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
              <div><span v-for="code in ['C', 'Java', 'JavaScript', 'PHP', 'Vue', 'SwiftUI']" :key="code">{{ code }}</span></div>
            </div>
          </div>
          <figure class="portrait-card">
            <img src="/assets/rex-portrait.png" alt="Rex in Tokyo at night" />
            <figcaption>
              <a href="https://x.com/Xer_Gnik" target="_blank" rel="noreferrer">X <span>Xer_Gnik</span></a>
              <a href="https://github.com/Rexking624" target="_blank" rel="noreferrer">GitHub <span>Rexking624</span></a>
              <a href="https://www.instagram.com/RexKing624" target="_blank" rel="noreferrer">Instagram <span>RexKing624</span></a>
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

        <article v-for="(project, index) in projects" :key="project.title" :id="project.title === 'EmotionChat' ? 'emotionchat' : project.title === 'XerFilm GR' ? 'xerfilm-gr' : undefined" class="project reveal-project" :class="{ 'project--triptych': project.title === 'ONE ARCANA', 'project--xerfilm': project.title === 'XerFilm GR' }" :style="{ '--reveal-delay': `${index * 70}ms` }">
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
            <div v-if="project.links.length" class="project-actions">
              <a v-for="link in project.links" :key="link[1]" class="project-action" :href="link[1]" target="_blank" rel="noreferrer">{{ link[0] }} <span>↗</span></a>
            </div>
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
