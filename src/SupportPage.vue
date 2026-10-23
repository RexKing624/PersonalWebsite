<script setup>
import { computed, onBeforeUnmount, onMounted, ref, watch } from 'vue'

const translations = {
  en: {
    language: 'Choose language', back: 'Back home', kicker: 'SUPPORT MY WORK', title: 'Support my work.',
    lead: 'If my projects, research, or images have inspired you in some small way, you can buy me a coffee.',
    paymentLabel: 'Payment methods', alipay: 'Alipay', wechat: 'WeChat Pay', paypayRegion: 'Japan',
    view: 'View ', close: 'Close ', paymentInfo: ' payment details', copy: 'Copy ID', copied: 'Copied',
    paypayHelp: 'Search this ID in PayPay', scanPrefix: 'Scan with ', thanks: 'Thank you for supporting independent work.',
  },
  ja: {
    language: '言語を選択', back: 'ホームへ戻る', kicker: 'SUPPORT MY WORK', title: '活動を支援する。',
    lead: 'プロジェクトや研究、写真から何かを感じてもらえたなら、コーヒーを一杯ご支援いただけるとうれしいです。',
    paymentLabel: '支払い方法', alipay: 'Alipay', wechat: 'WeChat Pay', paypayRegion: '日本',
    view: '表示', close: '閉じる', paymentInfo: 'の支払い情報', copy: 'IDをコピー', copied: 'コピーしました',
    paypayHelp: 'PayPayでこのIDを検索', scanPrefix: 'スキャン：', thanks: '個人制作へのご支援、ありがとうございます。',
  },
  zh: {
    language: '语言选择', back: '回到主页', kicker: 'SUPPORT MY WORK', title: '支持我的创作。',
    lead: '如果我的项目、研究或影像曾给你带来一点启发，可以请我喝一杯咖啡。',
    paymentLabel: '付款方式', alipay: '支付宝', wechat: '微信支付', paypayRegion: '日本',
    view: '查看', close: '关闭', paymentInfo: '付款信息', copy: '复制 ID', copied: '已复制',
    paypayHelp: '在 PayPay 中搜索此 ID', scanPrefix: '请使用 ', thanks: 'Thank you for supporting independent work.',
  },
}

const browserLanguage = (navigator.language || '').toLowerCase()
const language = ref(browserLanguage.startsWith('zh') ? 'zh' : browserLanguage.startsWith('ja') ? 'ja' : 'en')
const t = computed(() => translations[language.value])
const methods = computed(() => [
  { id: 'alipay', name: t.value.alipay, englishName: 'Alipay', accent: '#1677ff', kind: 'qr', qr: '/assets/support/alipay-qr.png' },
  { id: 'wechat', name: t.value.wechat, englishName: 'WeChat Pay', accent: '#07c160', kind: 'qr', qr: '/assets/support/wechat-pay-qr.png' },
  { id: 'paypay', name: 'PayPay', englishName: t.value.paypayRegion, accent: '#ff003c', kind: 'id', paymentId: 'rexking624' },
])

const hovered = ref(null)
const pinned = ref(null)
const missingImages = ref(new Set())
const copiedId = ref(null)
const visibleMethod = computed(() => pinned.value || hovered.value)
const previousTitle = document.title
let copyTimer

const canHover = () => window.matchMedia('(hover: hover) and (pointer: fine)').matches
const revealOnHover = (id) => {
  if (canHover()) hovered.value = id
}
const hideOnHover = (id) => {
  if (hovered.value === id) hovered.value = null
}
const toggleMethod = (id) => {
  pinned.value = pinned.value === id ? null : id
}
const registerMissingImage = (id) => {
  missingImages.value = new Set([...missingImages.value, id])
}
const copyPaymentId = async (method) => {
  try {
    await navigator.clipboard.writeText(method.paymentId)
    copiedId.value = method.id
    window.clearTimeout(copyTimer)
    copyTimer = window.setTimeout(() => { copiedId.value = null }, 1600)
  } catch {
    copiedId.value = null
  }
}
const closeWithEscape = (event) => {
  if (event.key === 'Escape') {
    pinned.value = null
    hovered.value = null
  }
}

watch(pinned, (value) => {
  if (!canHover()) document.body.style.overflow = value ? 'hidden' : ''
})

watch(language, (value) => {
  document.documentElement.lang = value === 'zh' ? 'zh-CN' : value
  document.title = `${translations[value].title.replace('。', '')} — XerGnik`
}, { immediate: true })

onMounted(() => {
  window.addEventListener('keydown', closeWithEscape)
})
onBeforeUnmount(() => {
  document.title = previousTitle
  document.body.style.overflow = ''
  window.clearTimeout(copyTimer)
  window.removeEventListener('keydown', closeWithEscape)
})
</script>

<template>
  <main class="support-page">
    <header class="support-nav">
      <a class="support-mark" href="/" :aria-label="t.back">
        <img src="/assets/logo-b.png" alt="Rex" />
      </a>
      <div class="support-nav-actions">
        <div class="support-language" role="group" :aria-label="t.language">
          <button v-for="item in [{ id: 'en', label: 'EN' }, { id: 'ja', label: '日' }, { id: 'zh', label: '中' }]" :key="item.id" type="button" :class="{ active: language === item.id }" :aria-pressed="language === item.id" @click="language = item.id">{{ item.label }}</button>
        </div>
        <a class="support-back" href="/">{{ t.back }} <span>↗</span></a>
      </div>
    </header>

    <section class="support-intro" aria-labelledby="support-title">
      <p class="support-kicker">{{ t.kicker }}</p>
      <h1 id="support-title">{{ t.title }}</h1>
      <p class="support-lead">{{ t.lead }}</p>
    </section>

    <section class="payment-grid" :aria-label="t.paymentLabel">
      <article
        v-for="(method, index) in methods"
        :key="method.id"
        class="payment-card"
        :class="{ 'is-visible': visibleMethod === method.id }"
        :style="{ '--payment-accent': method.accent }"
        @mouseenter="revealOnHover(method.id)"
        @mouseleave="hideOnHover(method.id)"
      >
        <button
          type="button"
          class="payment-trigger"
          :aria-expanded="visibleMethod === method.id"
          :aria-controls="`${method.id}-qr`"
          :aria-label="`${visibleMethod === method.id ? t.close : t.view}${method.name}${t.paymentInfo}`"
          @click="toggleMethod(method.id)"
        >
          <span class="payment-index">0{{ index + 1 }}</span>
          <span class="payment-name">
            <strong>{{ method.name }}</strong>
            <small>{{ method.englishName }}</small>
          </span>
          <span class="payment-hint">
            <i aria-hidden="true">＋</i>
          </span>
        </button>

        <div
          :id="`${method.id}-qr`"
          class="qr-reveal"
          :aria-hidden="visibleMethod !== method.id"
          :role="visibleMethod === method.id ? 'dialog' : undefined"
          :aria-modal="visibleMethod === method.id && !canHover() ? 'true' : undefined"
          @click.self="pinned = null"
        >
          <div class="qr-reveal-inner">
            <div v-if="method.kind === 'id'" class="payment-id-panel">
              <span>PayPay ID</span>
              <strong>{{ method.paymentId }}</strong>
              <button type="button" @click="copyPaymentId(method)">
                {{ copiedId === method.id ? t.copied : t.copy }}
              </button>
            </div>
            <div v-else class="qr-frame">
              <img
                v-if="visibleMethod === method.id && !missingImages.has(method.id)"
                :src="method.qr"
                :alt="`${method.name}付款码`"
                @error="registerMissingImage(method.id)"
              />
              <div v-else-if="visibleMethod === method.id" class="qr-placeholder">
                <span>付款码待放入</span>
                <code>{{ method.qr }}</code>
              </div>
            </div>
            <p v-if="method.kind === 'id'">{{ t.paypayHelp }}</p>
            <p v-else>{{ t.scanPrefix }}{{ method.name }}</p>
          </div>
        </div>
      </article>
    </section>

    <footer class="support-footer">
      <p>{{ t.thanks }}</p>
      <p>© 2026 RexKing624</p>
    </footer>
  </main>
</template>

<style scoped>
.support-page {
  min-height: 100svh;
  max-width: 1500px;
  margin: 0 auto;
  padding: 0 4vw;
  color: #f1efe8;
  background: #151614;
}

.support-nav {
  height: 104px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid rgba(241, 239, 232, .18);
}

.support-mark {
  display: grid;
  place-items: center;
  width: 62px;
  height: 62px;
  padding: 8px;
  border-radius: 50%;
  background: #f1efe8;
}

.support-mark img { display: block; width: 100%; height: 100%; object-fit: contain; }
.support-nav-actions { display: flex; align-items: center; gap: clamp(22px, 3vw, 46px); }
.support-language { display: flex; align-items: center; gap: 3px; padding-right: clamp(18px, 2vw, 30px); border-right: 1px solid rgba(241, 239, 232, .18); }
.support-language button { width: 31px; height: 31px; padding: 0; border: 0; border-radius: 50%; color: #777970; background: transparent; font-size: 10px; cursor: pointer; }
.support-language button.active { color: #151614; background: #f1efe8; }
.support-back { display: inline-flex; gap: 18px; padding-bottom: 6px; border-bottom: 1px solid; font-size: 13px; }
.support-back span { color: #3157c8; }

.support-intro { padding: clamp(84px, 11vw, 160px) 0 clamp(64px, 8vw, 110px); }
.support-kicker { margin: 0 0 30px; color: #777970; font-size: 11px; letter-spacing: .16em; }
.support-intro h1 {
  max-width: 1000px;
  margin: 0;
  font-family: 'Noto Serif SC', serif;
  font-size: clamp(58px, 9vw, 136px);
  font-weight: 500;
  line-height: .98;
  letter-spacing: -.055em;
}
.support-lead { max-width: 520px; margin: 46px 0 0 auto; color: #aaa99f; font-size: 15px; line-height: 1.9; }

.payment-grid { border-top: 1px solid rgba(241, 239, 232, .18); }
.payment-card { position: relative; border-bottom: 1px solid rgba(241, 239, 232, .18); }
.payment-card::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  width: 3px;
  height: 100%;
  background: var(--payment-accent);
  transform: scaleY(0);
  transform-origin: bottom;
  transition: transform .28s ease;
}
.payment-card.is-visible::before { transform: scaleY(1); }

.payment-trigger {
  width: 100%;
  min-height: 122px;
  display: grid;
  grid-template-columns: 90px 1fr auto;
  align-items: center;
  gap: 28px;
  padding: 26px 0;
  border: 0;
  color: inherit;
  background: transparent;
  text-align: left;
  cursor: pointer;
}
.payment-index { padding-left: 18px; color: #777970; font-size: 11px; letter-spacing: .13em; }
.payment-name { display: flex; align-items: baseline; gap: 20px; }
.payment-name strong { font-family: 'Noto Serif SC', serif; font-size: clamp(28px, 3vw, 44px); font-weight: 500; }
.payment-name small { color: #777970; font-size: 12px; }
.payment-hint { display: flex; align-items: center; color: #aaa99f; }
.payment-hint i { color: var(--payment-accent); font-size: 24px; font-style: normal; transition: transform .25s ease; }
.payment-card.is-visible .payment-hint i { transform: rotate(45deg); }

.qr-reveal {
  position: absolute;
  z-index: 10;
  right: 70px;
  top: 50%;
  width: min(310px, 32vw);
  opacity: 0;
  visibility: hidden;
  pointer-events: none;
  transform: translateY(-50%) scale(.96);
  transition: opacity .22s ease, transform .3s cubic-bezier(.22,.61,.36,1), visibility 0s linear .3s;
}
.payment-card.is-visible { z-index: 4; }
.payment-card.is-visible .qr-reveal {
  opacity: 1;
  visibility: visible;
  pointer-events: auto;
  transform: translateY(-50%) scale(1);
  transition-delay: 0s;
}
.qr-reveal-inner {
  padding: 14px 14px 1px;
  border: 1px solid rgba(241, 239, 232, .18);
  border-radius: 8px;
  background: #0d0e0c;
  box-shadow: 0 26px 80px rgba(0, 0, 0, .48);
}
.qr-frame {
  width: 100%;
  aspect-ratio: 1;
  margin: 0 auto 14px;
  padding: 18px;
  border-radius: 4px;
  background: #f5f3ec;
}
.qr-frame img { display: block; width: 100%; height: 100%; object-fit: contain; }
.qr-placeholder { height: 100%; display: grid; place-content: center; gap: 12px; color: #171816; text-align: center; }
.qr-placeholder span { font-family: 'Noto Serif SC', serif; font-size: 22px; }
.qr-placeholder code { max-width: 240px; color: #777970; font: 10px/1.5 'DM Sans', sans-serif; overflow-wrap: anywhere; }
.qr-reveal-inner > p { margin: 0 0 13px; color: #777970; font-size: 11px; text-align: center; letter-spacing: .08em; }
.payment-id-panel {
  min-height: 260px;
  display: grid;
  place-content: center;
  justify-items: center;
  gap: 18px;
  border-radius: 4px;
  color: #171816;
  background: #f5f3ec;
}
.payment-id-panel > span { color: #777970; font-size: 11px; letter-spacing: .14em; text-transform: uppercase; }
.payment-id-panel strong { font-family: 'Noto Serif SC', serif; font-size: 30px; font-weight: 500; letter-spacing: .02em; }
.payment-id-panel button {
  padding: 8px 18px;
  border: 1px solid rgba(23, 24, 22, .22);
  border-radius: 999px;
  color: #171816;
  background: transparent;
  font-size: 11px;
  cursor: pointer;
}
.payment-link {
  display: flex;
  justify-content: center;
  gap: 12px;
  margin: -2px 0 13px;
  color: #aaa99f;
  font-size: 11px;
}
.payment-link span { color: #0070e0; }

.support-footer {
  min-height: 150px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 30px;
  color: #777970;
  font-size: 11px;
}

@media (max-width: 760px) {
  .support-page { padding: 0 max(20px, env(safe-area-inset-left)) 0 max(20px, env(safe-area-inset-right)); }
  .support-nav { height: 80px; }
  .support-mark { width: 48px; height: 48px; padding: 6px; }
  .support-nav-actions { gap: 14px; }
  .support-language { gap: 0; padding-right: 12px; }
  .support-language button { width: 27px; height: 27px; font-size: 9px; }
  .support-intro { padding: 78px 0 64px; }
  .support-intro h1 { font-size: clamp(52px, 17vw, 78px); }
  .support-lead { margin: 32px 0 0; }
  .payment-trigger { min-height: 108px; grid-template-columns: 38px 1fr auto; gap: 12px; }
  .payment-index { padding-left: 12px; }
  .payment-name { display: grid; gap: 4px; }
  .payment-name strong { font-size: 27px; }
  .qr-reveal {
    position: fixed;
    inset: 0;
    min-height: 100dvh;
    width: auto;
    display: grid;
    place-items: center;
    padding: max(24px, env(safe-area-inset-top)) max(20px, env(safe-area-inset-right)) max(24px, env(safe-area-inset-bottom)) max(20px, env(safe-area-inset-left));
    background: rgba(9, 10, 9, .76);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    transform: scale(.98);
    overflow-y: auto;
    overscroll-behavior: contain;
  }
  .payment-card.is-visible .qr-reveal { transform: scale(1); }
  .qr-reveal-inner { width: min(380px, 100%); max-height: calc(100dvh - 48px); overflow-y: auto; }
  .support-footer { min-height: 130px; align-items: flex-start; flex-direction: column; justify-content: center; gap: 8px; }
  .support-footer p { margin: 0; }
}

@media (max-width: 380px) {
  .support-page { padding-left: 16px; padding-right: 16px; }
  .support-back { gap: 10px; font-size: 12px; }
  .support-intro h1 { font-size: 48px; }
  .payment-trigger { grid-template-columns: 34px minmax(0, 1fr) 24px; gap: 8px; }
  .payment-index { padding-left: 10px; }
  .payment-name strong { font-size: 24px; }
  .payment-name small { font-size: 10px; }
  .qr-frame { padding: 12px; }
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { transition-duration: .01ms !important; }
}
</style>
