<script setup>
import { lpContent } from '../../data/lpContent.js'
import { img } from '../../utils/image.js'
import SectionHeading from './SectionHeading.vue'
import IllustrationFrame from '../common/IllustrationFrame.vue'

const { supportSection: section, supportItems } = lpContent
</script>

<template>
  <section id="support" class="support lp-section lp-section--cream" aria-labelledby="support-title">
    <img class="lp-deco support__dots" :src="img('common/deco-dots.svg')" alt="" aria-hidden="true" />

    <div class="lp-container">
      <SectionHeading
        id="support-title"
        :number="section.number"
        :label="section.label"
        :title="section.title"
        :lead="section.lead"
      />

      <div class="support__grid">
        <ul class="support__list">
          <!-- 外側：スクロール表示 / 中間：浮遊アニメーション / 内側：カードのデザインと hover -->
          <li
            v-for="(item, index) in supportItems"
            :key="item.title"
            v-reveal="{ delay: 100, i: index }"
            class="support__item"
          >
            <div class="support__float" :class="`support__float--${index % 4}`">
              <div class="support-card lp-card" :class="`accent-${item.accent || 'yellow'}`">
                <div class="support-card__icon">
                  <img :src="img(item.icon)" alt="" width="120" height="120" loading="lazy" />
                </div>
                <h3 class="support-card__title">{{ item.title }}</h3>
                <p class="support-card__text">{{ item.description }}</p>
              </div>
            </div>
          </li>
        </ul>

        <figure v-reveal="{ variant: 'right', delay: 200 }" class="support__visual">
          <span class="lp-blob lp-blob--yellow support__visual-blob" aria-hidden="true"></span>
          <!-- 画像は共通の表示枠（背景透過PNG推奨）。後ろの丸い背景はCSSの装飾 -->
          <!-- 外側(figure)：スクロール表示 / 中間：浮遊アニメーション / 内側：画像枠 -->
          <!-- 人物画像と同じ幅の舞台：下の光 → 人物 → 上の葉っぱ の順に重ねる（装飾は人物と別レイヤー） -->
          <div class="support__stage">
            <!-- 下側：淡い光・小さなドット・手書き風の短い線 -->
            <div class="support-deco-bottom" aria-hidden="true">
              <span class="sd-glow"></span>
              <span class="sd-dot sd-dot--1"></span>
              <span class="sd-dot sd-dot--2"></span>
              <span class="sd-dot sd-dot--3"></span>
              <span class="sd-dot sd-dot--4"></span>
              <span class="sd-dot sd-dot--5"></span>
              <span class="sd-dot sd-dot--6"></span>
              <svg class="sd-lines" viewBox="0 0 300 60" preserveAspectRatio="none" focusable="false">
                <path class="sd-line sd-line--yellow" pathLength="1" d="M18 30 C 40 20, 62 20, 84 30" />
                <path class="sd-line sd-line--mint" pathLength="1" d="M112 46 C 140 36, 170 50, 198 40 C 214 34, 226 36, 236 42" />
                <path class="sd-line sd-line--blue" pathLength="1" d="M226 18 C 244 10, 262 12, 282 20" />
              </svg>
            </div>

            <div class="support__illustration-float">
              <IllustrationFrame class="support__frame" :src="section.image" :alt="section.imageAlt" ratio="3 / 2" />
            </div>

            <!-- 上側：ゆっくり揺れる葉っぱ（既存の素材を再利用） -->
            <div class="support-deco-top" aria-hidden="true">
              <img class="sd-leaf sd-leaf--1" :src="img('common/deco-leaf-pair.svg')" alt="" />
              <img class="sd-leaf sd-leaf--2" :src="img('common/deco-leaf.svg')" alt="" />
              <img class="sd-leaf sd-leaf--3" :src="img('common/deco-leaf.svg')" alt="" />
              <img class="sd-leaf sd-leaf--4" :src="img('common/deco-leaf.svg')" alt="" />
            </div>
          </div>
        </figure>
      </div>
    </div>
  </section>
</template>

<style scoped>
.support__grid {
  display: grid;
  grid-template-columns: 1.4fr 1fr;
  gap: clamp(32px, 4vw, 64px);
  align-items: center;
  margin-top: var(--space-xl);
}
.support__list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
}
.support {
  /* 浮遊の強さ（Tablet / SP で弱める） */
  --card-float-y: 7px;
  --card-float-x: 2px;
  --card-float-r: 0.35deg;
  --img-float-y: 8px;
  --img-float-x: 4px;
  --img-float-r: 0.4deg;
}
/* カードの高さを行ごとに揃えたまま、3層に分ける */
.support__item,
.support__float {
  display: flex;
}
.support__float,
.support-card {
  flex: 1;
}
.support__float {
  will-change: transform;
  animation: support-float-a 7.2s ease-in-out infinite;
}
/* 4枚が同時に同じ方向へ動かないよう、パターン・周期・開始をずらす */
.support__float--1 { animation-name: support-float-b; animation-duration: 8.4s; animation-delay: -4.6s; }
.support__float--2 { animation-name: support-float-c; animation-duration: 7.8s; animation-delay: -2.4s; }
.support__float--3 { animation-name: support-float-a; animation-duration: 9s; animation-delay: -6.2s; animation-direction: reverse; }

.support-card {
  padding: 26px 24px;
  background: var(--accent-pale);
  /* 背景からふわっと浮いて見える柔らかい影（静的） */
  box-shadow: 0 14px 32px rgba(31, 54, 105, 0.07), 0 5px 12px rgba(31, 54, 105, 0.04);
  border: 2px solid var(--color-white);
  transition: transform 0.35s cubic-bezier(0.22, 0.61, 0.36, 1), box-shadow 0.35s ease;
}
@media (hover: hover) and (pointer: fine) {
  .support-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 20px 40px rgba(31, 54, 105, 0.1), 0 7px 16px rgba(31, 54, 105, 0.05);
  }
}

@keyframes support-float-a {
  0%, 100% { transform: translate3d(0, 0, 0) rotate(0deg); }
  35% { transform: translate3d(var(--card-float-x), calc(var(--card-float-y) * -1), 0) rotate(var(--card-float-r)); }
  70% { transform: translate3d(calc(var(--card-float-x) * -0.5), calc(var(--card-float-y) * -0.45), 0) rotate(calc(var(--card-float-r) * -0.6)); }
}
@keyframes support-float-b {
  0%, 100% { transform: translate3d(0, 0, 0) rotate(0deg); }
  40% { transform: translate3d(calc(var(--card-float-x) * -1), calc(var(--card-float-y) * -0.8), 0) rotate(calc(var(--card-float-r) * -1)); }
  75% { transform: translate3d(calc(var(--card-float-x) * 0.6), calc(var(--card-float-y) * -1), 0) rotate(calc(var(--card-float-r) * 0.6)); }
}
@keyframes support-float-c {
  0%, 100% { transform: translate3d(0, 0, 0) rotate(0deg); }
  30% { transform: translate3d(calc(var(--card-float-x) * 0.5), calc(var(--card-float-y) * -0.6), 0) rotate(calc(var(--card-float-r) * 0.6)); }
  60% { transform: translate3d(calc(var(--card-float-x) * -1), calc(var(--card-float-y) * -1), 0) rotate(calc(var(--card-float-r) * -1)); }
}
.support-card__icon {
  width: 64px;
  height: 64px;
  margin-bottom: 12px;
}
.support-card__icon img {
  width: 100%;
  height: 100%;
}
.support-card__title {
  font-size: var(--fs-lg);
  font-weight: 900;
}
.support-card__text {
  margin-top: 8px;
  font-size: var(--fs-sm);
  line-height: 1.85;
}
.support__visual {
  position: relative;
}

/* ================= 右の人物イラスト周辺の装飾 ================= */
.support__stage {
  --sd-amp: 1; /* 動く量の倍率（SPで弱める） */
  position: relative;
  max-width: 440px;
  margin-inline: auto;
}
.support-deco-top,
.support-deco-bottom {
  position: absolute;
  pointer-events: none;
}
.support-deco-top > *,
.support-deco-bottom > * {
  position: absolute;
  pointer-events: none;
  user-select: none;
}
/* 葉っぱ：人物の上に置くレイヤー。顔にかからない上側〜左右上に散らす */
.support-deco-top {
  inset: 0;
  z-index: 3;
}
.sd-leaf {
  transform-origin: 50% 90%;
  animation: sd-leaf-sway var(--dur, 7s) ease-in-out var(--delay, 0s) infinite;
}
.sd-leaf--1 { --dur: 6.5s; --delay: -1s;   width: 58px; left: -4%; top: -16%; rotate: -18deg; opacity: 0.75; }
.sd-leaf--2 { --dur: 8s;   --delay: -3.5s; width: 26px; left: 36%; top: -13%; rotate: 24deg;  opacity: 0.55; filter: blur(0.3px); }
.sd-leaf--3 { --dur: 7.2s; --delay: -2s;   width: 38px; right: 4%; top: -14%; rotate: 32deg;  opacity: 0.8; }
.sd-leaf--4 { --dur: 9s;   --delay: -5s;   width: 22px; right: -6%; top: 20%; rotate: -8deg;  opacity: 0.45; }
@keyframes sd-leaf-sway {
  0%, 100% { transform: translate3d(0, 0, 0) rotate(-4deg); }
  35% { transform: translate3d(calc(9px * var(--sd-amp)), calc(-10px * var(--sd-amp)), 0) rotate(7deg); }
  70% { transform: translate3d(calc(-4px * var(--sd-amp)), calc(-4px * var(--sd-amp)), 0) rotate(-7deg); }
}

/* 下側：人物の後ろ（奥）のレイヤー */
.support-deco-bottom {
  left: 4%;
  right: 4%;
  bottom: -34px;
  height: 84px;
  z-index: 0;
}
/* 横長の淡い光：左 → 中央 → 右 → 中央 とゆっくり漂う */
.sd-glow {
  inset: 10% -6% -6%;
  border-radius: 50%;
  background: radial-gradient(ellipse at center, rgba(255, 220, 120, 0.3), rgba(150, 230, 200, 0.18) 45%, rgba(190, 222, 250, 0.1) 60%, transparent 72%);
  animation: sd-glow-drift 10s ease-in-out infinite;
}
@keyframes sd-glow-drift {
  0%, 100% { transform: translateX(-4%) scale(0.96); opacity: 0.55; }
  50% { transform: translateX(4%) scale(1.05); opacity: 0.9; }
}
/* 小さなドット：浮かんで横へ流れ、薄くなってまた現れる */
.sd-dot {
  width: var(--s, 5px);
  height: var(--s, 5px);
  border-radius: 50%;
  background: var(--c);
  animation: sd-dot-drift var(--dur, 5s) ease-in-out var(--delay, 0s) infinite;
}
.sd-dot--1 { --s: 6px; --c: #ffd95a; --dur: 5s;   --delay: 0s;    left: 12%; top: 30%; }
.sd-dot--2 { --s: 4px; --c: #7fd6b8; --dur: 6.2s; --delay: -1.5s; left: 30%; top: 62%; }
.sd-dot--3 { --s: 7px; --c: #8fbdf0; --dur: 4.5s; --delay: -3s;   left: 52%; top: 22%; }
.sd-dot--4 { --s: 3px; --c: #f58a80; --dur: 7s;   --delay: -2s;   left: 68%; top: 58%; }
.sd-dot--5 { --s: 5px; --c: #7fd6b8; --dur: 5.6s; --delay: -4s;   left: 84%; top: 34%; }
.sd-dot--6 { --s: 4px; --c: #ffd95a; --dur: 6.6s; --delay: -0.8s; left: 92%; top: 66%; }
@keyframes sd-dot-drift {
  0%, 100% { transform: translate3d(0, calc(5px * var(--sd-amp)), 0); opacity: 0.25; }
  50% { transform: translate3d(calc(5px * var(--sd-amp)), calc(-8px * var(--sd-amp)), 0); opacity: 0.8; }
}
/* 手書き風の短い線：左→右へ描く → 少し維持 → 消える → また描く */
.sd-lines {
  inset: 0;
  width: 100%;
  height: 100%;
  overflow: visible;
}
.sd-line {
  fill: none;
  stroke-width: 2.5;
  stroke-linecap: round;
  vector-effect: non-scaling-stroke;
  stroke-dasharray: 1;
  stroke-dashoffset: 0;
  animation: sd-line-draw 6.5s ease-in-out var(--delay, 0s) infinite both;
}
.sd-line--yellow { --delay: 0.3s; stroke: #f6d24a; }
.sd-line--mint   { --delay: 1s;   stroke: #6fd0b2; }
.sd-line--blue   { --delay: 1.7s; stroke: #7fc0ee; }
@keyframes sd-line-draw {
  0%, 4% { stroke-dashoffset: 1; opacity: 0; }
  5% { opacity: 0.85; }
  22% { stroke-dashoffset: 0; opacity: 0.85; }
  62% { stroke-dashoffset: 0; opacity: 0.85; }
  74% { stroke-dashoffset: 0; opacity: 0; }
  75%, 100% { stroke-dashoffset: 1; opacity: 0; }
}
/* 右の画像：カードよりさらにゆっくり（translate＋ごく小さい rotate のみ・scale なし） */
.support__illustration-float {
  position: relative;
  z-index: 2;
  will-change: transform;
  /* 背景透過PNG向けの柔らかい影（白背景入りPNGの場合は四角い影になるため、素材は背景透過を推奨） */
  filter: drop-shadow(0 16px 24px rgba(31, 54, 105, 0.08));
  animation: support-img-float 12s ease-in-out -3s infinite;
}
@keyframes support-img-float {
  0%, 100% { transform: translate3d(0, 0, 0) rotate(calc(var(--img-float-r) * -0.75)); }
  35% { transform: translate3d(var(--img-float-x), calc(var(--img-float-y) * -1), 0) rotate(var(--img-float-r)); }
  70% { transform: translate3d(calc(var(--img-float-x) * -0.5), calc(var(--img-float-y) * -0.375), 0) rotate(0deg); }
}
.support__frame {
  position: relative;
  z-index: 1;
  max-width: 440px;
  margin-inline: auto;
}
.support__visual-blob {
  inset: 8% 4%;
}
.support__dots {
  width: 96px;
  right: 6%;
  top: 80px;
}

/* Tablet：少し弱く */
@media (max-width: 1024px) {
  .support {
    --card-float-y: 5px;
    --card-float-r: 0.3deg;
    --img-float-y: 6px;
    --img-float-x: 3px;
  }
}
@media (max-width: 960px) {
  .support__grid {
    grid-template-columns: 1fr;
  }
  .support__visual {
    order: -1;
    width: 100%; /* 中央寄せでも画像枠が幅を持つように（画像の大きさに依存しない） */
    max-width: 360px;
    margin-inline: auto;
  }
}
/* SP：さらに弱く（読みやすさ優先） */
@media (max-width: 767px) {
  .support {
    --card-float-y: 4px;
    --card-float-x: 1px;
    --card-float-r: 0.15deg;
    --img-float-y: 5px;
    --img-float-x: 2px;
    --img-float-r: 0.2deg;
  }
  .support__float,
  .support__illustration-float {
    will-change: auto;
  }
  /* 人物まわりの装飾：数を減らし、動く量を約25%小さく */
  .support__stage {
    --sd-amp: 0.75;
  }
  .sd-leaf--2,
  .sd-leaf--4,
  .sd-dot--2,
  .sd-dot--5 {
    display: none;
  }
  .sd-leaf--1 { width: 46px; }
  /* 画像とカードの間の余白に収める */
  .support-deco-bottom {
    bottom: -18px;
    height: 46px;
  }
  .sd-glow {
    inset: 10% 0 0;
  }
}
@media (max-width: 600px) {
  .support__list {
    grid-template-columns: 1fr;
  }
  .support-card {
    display: grid;
    grid-template-columns: 56px 1fr;
    column-gap: 16px;
    padding: 20px;
  }
  .support-card__icon {
    grid-row: span 2;
    width: 56px;
    height: 56px;
    margin: 0;
  }
  .support-card__text {
    margin-top: 4px;
  }
}

@media (prefers-reduced-motion: reduce) {
  .sd-leaf,
  .sd-glow,
  .sd-dot,
  .sd-line {
    animation: none !important;
  }
  .sd-dot {
    opacity: 0.6;
  }
  .sd-line {
    stroke-dashoffset: 0;
    opacity: 0.85;
  }
  .support__float,
  .support__illustration-float {
    animation: none !important;
    transform: none !important;
    will-change: auto;
  }
  .support-card {
    transition: none;
  }
  .support-card:hover {
    transform: none;
  }
}
</style>
