<script setup>
import { lpContent } from '../../data/lpContent.js'
import { img } from '../../utils/image.js'
import SectionHeading from './SectionHeading.vue'

const { selectionSection: section, selectionFlow } = lpContent
</script>

<template>
  <section id="selection" class="selection lp-section" aria-labelledby="selection-title">
    <span class="lp-blob lp-blob--yellow selection__blob" aria-hidden="true"></span>
    <span class="lp-blob lp-blob--pink selection__blob-pink" aria-hidden="true"></span>
    <img class="lp-deco selection__sparkle" :src="img('common/deco-sparkle.svg')" alt="" aria-hidden="true" />

    <div class="lp-container">
      <!-- 見出し：05 と同じ共通の中央揃え見出し -->
      <SectionHeading
        id="selection-title"
        :number="section.number"
        :label="section.label"
        :title="section.title"
        :lead="section.lead"
        align="center"
      />

      <ol class="flow">
        <!-- 画面に入ったら STEP 01 → 05 の順に表示（共通の v-reveal） -->
        <li
          v-for="(item, index) in selectionFlow"
          :key="item.title"
          v-reveal="{ variant: 'flow', delay: 100, i: index }"
          class="flow__step"
        >
          <div class="flow__circle" :class="`flow__circle--${item.circle || 'blue'}`">
            <img class="flow__icon" :src="img(item.icon)" alt="" width="120" height="120" loading="lazy" />
          </div>
          <div class="flow__body">
            <h3 class="flow__title">{{ item.title }}</h3>
            <p v-if="item.duration" class="flow__duration">（{{ item.duration }}）</p>
            <p v-if="item.note" class="flow__note pre-line">{{ item.note }}</p>
          </div>
          <!-- STEP間の小さなChevron（装飾） -->
          <span v-if="index < selectionFlow.length - 1" class="flow__chevron" aria-hidden="true"></span>
        </li>
      </ol>

      <p v-if="section.note" class="selection__note">{{ section.note }}</p>
    </div>
  </section>
</template>

<style scoped>
.selection {
  --flow-orange: #f5a15d;
  --flow-lavender: #efeafb;
  --circle-size: clamp(72px, 6.4vw, 88px);
  background: linear-gradient(180deg, #fffdf7 0%, var(--color-cream) 100%);
}

/* ---------- PC：5STEP横一列 ---------- */
.flow {
  --gap: clamp(20px, 3.2vw, 48px);
  display: grid;
  grid-template-columns: repeat(5, minmax(0, 1fr));
  column-gap: var(--gap);
  margin-top: clamp(40px, 4vw, 56px);
}
.flow__step {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}
.flow__circle {
  display: grid;
  place-items: center;
  width: var(--circle-size);
  height: var(--circle-size);
  border-radius: 50%;
  background: var(--circle-bg);
  box-shadow: 0 0 0 4px rgba(255, 255, 255, 0.85), 0 8px 20px rgba(31, 54, 105, 0.06);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.flow__circle--blue { --circle-bg: var(--color-blue-pale); }
.flow__circle--yellow { --circle-bg: var(--color-yellow-pale); }
.flow__circle--lavender { --circle-bg: var(--flow-lavender); }
.flow__circle--pink { --circle-bg: var(--color-pink-pale); }
.flow__circle--green { --circle-bg: var(--color-green-pale); }
.flow__icon {
  width: 58%;
  height: 58%;
  object-fit: contain;
}
.flow__body {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.flow__title {
  margin-top: 14px;
  color: var(--color-navy);
  font-size: clamp(0.9375rem, 0.8rem + 0.35vw, 1.125rem);
  font-weight: 700;
  letter-spacing: 0.04em;
  line-height: 1.4;
  white-space: nowrap;
}
.flow__duration {
  margin-top: 4px;
  color: var(--color-text-muted);
  font-size: 0.8125rem;
  line-height: 1.5;
}
/* 内定の黄色いメモ（少し浮いて見える） */
.flow__note {
  margin-top: 8px;
  padding: 6px 14px;
  border-radius: 12px;
  background: var(--color-yellow);
  color: var(--color-navy);
  font-size: 0.8125rem;
  font-weight: 800;
  line-height: 1.45;
  box-shadow: 0 8px 20px rgba(200, 160, 30, 0.14), 0 2px 5px rgba(200, 160, 30, 0.1);
  animation: flow-note-float 6s ease-in-out infinite;
}
@keyframes flow-note-float {
  0%, 100% { translate: 0 0; }
  50% { translate: 0 -4px; }
}

/* STEP間の小さなオレンジChevron（丸の中心の高さに配置） */
.flow__chevron {
  position: absolute;
  top: calc(var(--circle-size) / 2);
  right: calc(var(--gap) / -2);
  width: 9px;
  height: 9px;
  border-top: 2.5px solid var(--flow-orange);
  border-right: 2.5px solid var(--flow-orange);
  border-radius: 1px;
  transform: translate(50%, -50%) rotate(45deg);
}

/* PC（マウス操作）：hover で丸だけ少し浮く（クリック要素ではないので cursor は変えない） */
@media (hover: hover) and (pointer: fine) {
  .flow__step:hover .flow__circle {
    transform: translateY(-3px);
    box-shadow: 0 0 0 4px rgba(255, 255, 255, 0.9), 0 12px 26px rgba(31, 54, 105, 0.1);
  }
}

/* ---------- 画面に入ったとき：Chevron は STEP より少し遅れて左から現れる ---------- */
.reveal-ready .flow__step.reveal .flow__chevron {
  opacity: 0;
  translate: -5px 0;
  transition: opacity 0.45s ease, translate 0.45s ease;
  transition-delay: calc(100ms * var(--reveal-base-scale) + var(--reveal-i, 0) * var(--reveal-step) + 220ms);
}
.reveal-ready .flow__step.is-revealed .flow__chevron {
  opacity: 1;
  translate: 0 0;
}

.selection__note {
  margin-top: clamp(28px, 3vw, 40px);
  color: var(--color-text-muted);
  font-size: var(--fs-xs);
  text-align: center;
}

/* ---------- 背景装飾（既存のブロブ＋小さな手書き風ライン） ---------- */
.selection__blob {
  width: 380px;
  height: 320px;
  left: -140px;
  top: -80px;
}
.selection__blob-pink {
  width: 260px;
  height: 220px;
  right: -90px;
  top: 40px;
  opacity: 0.8;
}
.selection__sparkle {
  width: 36px;
  right: 10%;
  top: 56px;
  opacity: 0.7;
}

/* ---------- Tablet：文字が重ならないよう詰める ---------- */
@media (max-width: 1024px) {
  .flow {
    --gap: 20px;
  }
  .flow__duration {
    font-size: 0.75rem;
  }
}

/* ---------- SP：縦型FLOW（丸は左・文字は右） ---------- */
@media (max-width: 767px) {
  .selection {
    --circle-size: 64px;
  }
  .selection__blob-pink {
    width: 180px;
    height: 160px;
    top: auto;
    bottom: 140px;
    right: -100px;
  }
  .flow {
    grid-template-columns: 1fr;
    row-gap: 0;
    max-width: 420px;
    margin-inline: auto;
  }
  .flow__step {
    display: grid;
    grid-template-columns: var(--circle-size) 1fr;
    column-gap: 18px;
    align-items: start;
    padding-bottom: 34px;
    text-align: left;
  }
  .flow__step:last-child {
    padding-bottom: 0;
  }
  .flow__body {
    align-items: flex-start;
    padding-top: 8px;
  }
  .flow__title {
    margin-top: 0;
    font-size: 1.0625rem;
  }
  .flow__duration {
    font-size: 0.8125rem;
  }
  /* 縦方向のつながり：細い線＋下向きの小さなChevron */
  .flow__step:not(:last-child)::before {
    content: '';
    position: absolute;
    left: calc(var(--circle-size) / 2 - 1px);
    top: calc(var(--circle-size) + 8px);
    bottom: 12px;
    width: 2px;
    border-radius: 1px;
    background: linear-gradient(to bottom, rgba(245, 161, 93, 0.15), rgba(245, 161, 93, 0.55));
  }
  .flow__chevron {
    top: auto;
    right: auto;
    left: calc(var(--circle-size) / 2);
    bottom: 8px;
    width: 8px;
    height: 8px;
    transform: translate(-50%, 0) rotate(135deg);
  }
  .reveal-ready .flow__step.reveal .flow__chevron {
    translate: 0 -5px;
  }
  .reveal-ready .flow__step.is-revealed .flow__chevron {
    translate: 0 0;
  }
  .flow__note {
    align-self: flex-start;
    animation-name: flow-note-float-sp;
  }
  @keyframes flow-note-float-sp {
    0%, 100% { translate: 0 0; }
    50% { translate: 0 -3px; }
  }
  .selection__sparkle {
    right: 16px;
    top: 24px;
  }
}

@media (prefers-reduced-motion: reduce) {
  .flow__note {
    animation: none;
  }
  .flow__circle,
  .flow__chevron {
    transition: none !important;
  }
  .reveal-ready .flow__step.reveal .flow__chevron {
    opacity: 1;
    translate: 0 0;
  }
  .flow__step:hover .flow__circle {
    transform: none;
  }
}
</style>
