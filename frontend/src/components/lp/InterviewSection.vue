<script setup>
import { lpContent } from '../../data/lpContent.js'
import { img } from '../../utils/image.js'
import SectionHeading from './SectionHeading.vue'
import IllustrationFrame from '../common/IllustrationFrame.vue'

const { interview } = lpContent
</script>

<template>
  <section id="interview" class="interview lp-section lp-section--cream" aria-labelledby="interview-title">
    <img class="lp-deco interview__dots" :src="img('common/deco-dots.svg')" alt="" aria-hidden="true" />

    <div class="lp-container interview__grid">
      <figure v-reveal="{ variant: 'left' }" class="interview__visual">
        <span class="lp-blob lp-blob--pink interview__visual-blob" aria-hidden="true"></span>
        <!-- 画像は共通の表示枠（背景透過PNG推奨）。後ろの丸い背景はCSSの装飾 -->
        <IllustrationFrame class="interview__frame" :src="interview.image" :alt="interview.imageAlt" ratio="6 / 5" />

        <!-- 手書き風メモ：外側＝スクロール表示 / 内側＝浮遊アニメーション（transform を分離） -->
        <div v-if="interview.note" v-reveal="{ delay: 250 }" class="interview__note-wrap">
          <p class="interview__note pre-line">{{ interview.note }}</p>
          <img class="interview__note-spark" :src="img('common/deco-sparkle.svg')" alt="" aria-hidden="true" />
        </div>
      </figure>

      <div class="interview__content">
        <SectionHeading
          id="interview-title"
          :number="interview.number"
          :label="interview.label"
          :title="interview.title"
          :lead="interview.lead"
        />

        <div v-reveal="{ delay: 200 }" class="interview__card lp-card">
          <h3 class="interview__list-title">{{ interview.listTitle }}</h3>
          <ul class="interview__topics">
            <li v-for="topic in interview.topics" :key="topic">{{ topic }}</li>
          </ul>
        </div>

        <ul v-if="interview.points?.length" v-reveal="{ delay: 300 }" class="interview__points">
          <li v-for="point in interview.points" :key="point">{{ point }}</li>
        </ul>
      </div>
    </div>
  </section>
</template>

<style scoped>
.interview__grid {
  display: grid;
  grid-template-columns: 1fr 1.1fr;
  gap: clamp(32px, 5vw, 80px);
  align-items: center;
}
.interview__visual {
  position: relative;
}
.interview__frame {
  position: relative;
  z-index: 1;
  max-width: 520px;
  margin-inline: auto;
}
.interview__visual-blob {
  inset: 4% 2%;
}

/* ---------- 手書き風メモ（01 の手書きメッセージと同じ書体・色） ---------- */
/* 画像枠を基準に %（右下）で配置するため、画像を差し替えても位置が崩れにくい。
   ブロック全体は右寄せ・中の2行は左揃え */
.interview__note-wrap {
  position: absolute;
  z-index: 2;
  left: auto;
  right: 8%;
  top: 100%;
  margin-top: -2.5%;
  pointer-events: none;
}
.interview__note {
  --note-y: 9px;
  --note-x: 3px;
  position: relative;
  color: #3a6db0; /* 01 の手書きメッセージと同じブルー */
  font-family: var(--font-heading);
  font-size: clamp(1rem, 0.85rem + 0.4vw, 1.1875rem);
  font-weight: 700;
  letter-spacing: 0.12em;
  line-height: 1.8;
  filter: drop-shadow(0 4px 8px rgba(31, 54, 105, 0.08));
  transform: rotate(-3deg);
  animation: interview-note-float 7s ease-in-out -1.5s infinite;
}
/* 淡い黄色の手書き風アンダーライン */
.interview__note::after {
  content: '';
  display: block;
  width: 78%;
  height: 7px;
  margin: -4px 0 0 18%;
  border-radius: 999px;
  background: var(--color-yellow);
  opacity: 0.45;
  transform: rotate(-2deg);
}
/* 右上の小さなアクセント線（既存の装飾画像） */
.interview__note-spark {
  position: absolute;
  top: -14px;
  right: -26px;
  width: 26px;
  opacity: 0.75;
  transform: rotate(18deg);
}
/* 空気の中にゆっくり浮いている動き（上下＋少し横＋わずかな傾き） */
@keyframes interview-note-float {
  0%, 100% { transform: translate3d(0, 0, 0) rotate(-3deg); }
  30% { transform: translate3d(var(--note-x), calc(var(--note-y) * -0.6), 0) rotate(-2deg); }
  65% { transform: translate3d(calc(var(--note-x) * -0.7), calc(var(--note-y) * -1), 0) rotate(-4deg); }
}
.interview__card {
  margin-top: var(--space-lg);
  padding: 28px 32px;
}
.interview__list-title {
  display: inline-block;
  padding: 0 4px;
  background: linear-gradient(transparent 60%, var(--color-yellow-pale) 60%);
  font-size: var(--fs-md);
  font-weight: 900;
}
.interview__topics {
  display: grid;
  gap: 12px;
  margin-top: 16px;
}
.interview__topics li {
  position: relative;
  padding-left: 36px;
  color: var(--color-navy);
  font-weight: 700;
  line-height: 1.6;
}
.interview__topics li::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: var(--color-green-pale);
}
.interview__topics li::after {
  content: '';
  position: absolute;
  left: 7px;
  top: 6px;
  width: 10px;
  height: 6px;
  border-left: 3px solid var(--color-coral);
  border-bottom: 3px solid var(--color-coral);
  transform: rotate(-45deg);
}
.interview__points {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 20px;
}
.interview__points li {
  padding: 6px 16px;
  border-radius: var(--radius-pill);
  background: var(--color-navy);
  color: var(--color-white);
  font-size: var(--fs-sm);
  font-weight: 700;
}
.interview__dots {
  width: 96px;
  left: 4%;
  bottom: 60px;
}

@media (max-width: 960px) {
  .interview__grid {
    grid-template-columns: 1fr;
  }
  .interview__visual {
    width: 100%; /* 中央寄せでも画像枠が幅を持つように（画像の大きさに依存しない） */
    max-width: 420px;
    margin-inline: auto;
    order: 1;
  }
  /* Tablet / SP：画像の下に縦に並べ、右寄せ（中の2行は左揃えのまま） */
  .interview__note-wrap {
    position: relative;
    left: auto;
    right: auto;
    top: auto;
    width: fit-content;
    margin: 6px 12% 0 auto;
  }
  .interview__note {
    --note-y: 5px;
    --note-x: 2px;
    font-size: 1rem;
  }
}
@media (max-width: 600px) {
  .interview__card {
    padding: 22px 20px;
  }
}

@media (prefers-reduced-motion: reduce) {
  .interview__note {
    animation: none;
  }
}
</style>
