<script setup>
import { img } from '../../utils/image.js'

defineProps({
  career: { type: Object, required: true },
  index: { type: Number, default: 0 },
})

// 左→右へ動きが伝わるように、周期と開始をカードごとにずらす（アイコンはカードの 0.7 秒後）
const DUR = [5.4, 5.8, 5.5, 6, 5.7]
</script>

<template>
  <article
    class="career-card lp-card"
    :class="`accent-${career.accent || 'yellow'}`"
    :style="{
      '--dur': `${DUR[index % DUR.length]}s`,
      '--delay': `${index * 0.5}s`,
      '--icon-delay': `${index * 0.5 + 0.7}s`,
      '--sweep-delay': `${index * 1.2 + 1}s`,
    }"
  >
    <!-- 淡い光の流れ（装飾・カード内に収める） -->
    <span class="career-card__sweep" aria-hidden="true"></span>
    <div class="career-card__icon">
      <img :src="img(career.icon)" alt="" width="120" height="120" loading="lazy" />
    </div>
    <h3 class="career-card__title">{{ career.title }}</h3>
    <p class="career-card__text">{{ career.description }}</p>
  </article>
</template>

<style scoped>
.career-card {
  --float: 5px; /* 浮く量（SPで弱める） */
  isolation: isolate; /* 下の光・光の流れの重なり順をカード内で完結させる */
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100%;
  padding: 28px 16px 24px;
  text-align: center;
  border-bottom: 6px solid var(--accent);
  /* 常にある柔らかい影＋下側のアクセント色のごく薄い光 */
  box-shadow:
    0 12px 30px rgba(35, 55, 95, 0.07),
    0 4px 12px rgba(35, 55, 95, 0.04),
    0 14px 28px -12px color-mix(in srgb, var(--accent) 45%, transparent);
  /* 常時の浮遊は translate、hover は transform（競合しない） */
  animation: career-float var(--dur, 5.5s) ease-in-out var(--delay, 0s) infinite;
  transition: transform 0.35s cubic-bezier(0.22, 0.61, 0.36, 1), box-shadow 0.35s ease;
}
/* 浮いたときに広がる影（濃さだけを浮遊と同じ周期で変える） */
.career-card::after {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  box-shadow:
    0 20px 42px rgba(35, 55, 95, 0.11),
    0 8px 18px rgba(35, 55, 95, 0.05),
    0 18px 30px -12px color-mix(in srgb, var(--accent) 60%, transparent);
  opacity: 0;
  pointer-events: none;
  animation: career-shadow var(--dur, 5.5s) ease-in-out var(--delay, 0s) infinite;
}
/* 下のアクセント色のライン付近にふんわりした光（ゆっくり呼吸） */
.career-card::before {
  content: '';
  position: absolute;
  z-index: -1;
  left: 12%;
  right: 12%;
  bottom: -14px;
  height: 18px;
  border-radius: 50%;
  background: radial-gradient(closest-side, color-mix(in srgb, var(--accent) 70%, transparent), transparent);
  opacity: 0.75;
  pointer-events: none;
  animation: career-accent-breathe 5.5s ease-in-out var(--delay, 0s) infinite;
}
/* 淡い光：左下 → 右上へゆっくり流れる（カードごとに時間差） */
.career-card__sweep {
  position: absolute;
  inset: 0;
  z-index: 0;
  overflow: hidden;
  border-radius: inherit;
  pointer-events: none;
}
.career-card__sweep::before {
  content: '';
  position: absolute;
  inset: -40% -60%;
  background: linear-gradient(115deg, transparent 42%, rgba(255, 255, 255, 0.55) 50%, transparent 58%);
  transform: translate3d(-60%, 30%, 0);
  animation: career-sweep 11s ease-in-out var(--sweep-delay, 1s) infinite both;
}
.career-card > :not(.career-card__sweep) {
  position: relative;
  z-index: 1;
}
.career-card__icon {
  width: 88px;
  height: 88px;
  margin-bottom: 14px;
  border-radius: 50%;
  /* カードより少し遅れて、小さく浮いて脈打つ（外側にごく薄い光） */
  animation: career-icon var(--dur, 5.5s) ease-in-out var(--icon-delay, 0.7s) infinite;
  transition: scale 0.35s ease;
}

/* PC（マウス操作）の hover：少しだけさらに持ち上がる */
@media (hover: hover) and (pointer: fine) {
  .career-card:hover {
    transform: translateY(-2px);
    box-shadow:
      0 18px 40px rgba(35, 55, 95, 0.1),
      0 6px 16px rgba(35, 55, 95, 0.05),
      0 18px 32px -12px color-mix(in srgb, var(--accent) 60%, transparent);
  }
  .career-card:hover .career-card__icon img {
    scale: 1.05;
  }
  .career-card__icon img {
    transition: scale 0.35s ease;
  }
}

@keyframes career-float {
  0%, 100% { translate: 0 0; }
  50% { translate: 0 calc(var(--float) * -1); }
}
@keyframes career-shadow {
  0%, 100% { opacity: 0; }
  50% { opacity: 1; }
}
@keyframes career-icon {
  0%, 100% { translate: 0 0; scale: 1; rotate: 0deg; box-shadow: 0 0 0 0 transparent; }
  50% {
    translate: 0 -2px;
    scale: 1.04;
    rotate: 1deg;
    box-shadow: 0 0 0 5px color-mix(in srgb, var(--accent) 12%, transparent), 0 7px 18px color-mix(in srgb, var(--accent) 28%, transparent);
  }
}
@keyframes career-accent-breathe {
  0%, 100% { opacity: 0.75; }
  50% { opacity: 1; }
}
@keyframes career-sweep {
  0% { transform: translate3d(-60%, 30%, 0); }
  30%, 100% { transform: translate3d(60%, -30%, 0); }
}
.career-card__icon img {
  width: 100%;
  height: 100%;
}
.career-card__title {
  font-size: var(--fs-md);
  font-weight: 900;
  line-height: 1.4;
}
.career-card__text {
  margin-top: 8px;
  color: var(--color-text);
  font-size: var(--fs-sm);
  line-height: 1.7;
}

@media (max-width: 767px) {
  .career-card {
    --float: 3px;
    box-shadow:
      0 8px 22px rgba(35, 55, 95, 0.06),
      0 3px 9px rgba(35, 55, 95, 0.035),
      0 12px 22px -12px color-mix(in srgb, var(--accent) 35%, transparent);
  }
}
@media (max-width: 600px) {
  /* 横長カード：アクセントの光を左側に */
  .career-card::before {
    left: -14px;
    right: auto;
    top: 15%;
    bottom: 15%;
    width: 18px;
    height: auto;
  }
  .career-card {
    display: grid;
    grid-template-columns: 64px 1fr;
    column-gap: 16px;
    align-items: center;
    padding: 18px 20px;
    text-align: left;
    border-bottom: 0;
    border-left: 6px solid var(--accent);
  }
  .career-card__icon {
    grid-row: span 2;
    width: 64px;
    height: 64px;
    margin: 0;
  }
  .career-card__text {
    margin-top: 2px;
  }
}

@media (prefers-reduced-motion: reduce) {
  .career-card,
  .career-card::after,
  .career-card::before,
  .career-card__icon,
  .career-card__sweep::before {
    animation: none !important;
  }
  .career-card__sweep {
    display: none;
  }
  .career-card {
    transition: none;
  }
  .career-card:hover,
  .career-card:hover .career-card__icon img {
    transform: none;
    scale: 1;
  }
}
</style>
