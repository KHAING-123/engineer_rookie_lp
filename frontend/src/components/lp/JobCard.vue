<script setup>
/**
 * 02 OUR WORK の仕事カード（1枚分）
 *  カードの色は lpContent.js の jobs[].accent（yellow / green / pink）で切り替わります。
 *  画像は IllustrationFrame で contain 表示（切り取り・色の加工なし）。PNG を差し替えても崩れません。
 *
 * レイヤーの分担（transform が競合しないように分離）
 *   li.our-work-card-reveal（JobsSection） … スクロール表示（1回だけ）
 *   .our-work-card                        … 常時のゆっくりした浮遊 ＋ 影の呼吸
 *   .our-work-card__surface               … カードの見た目と hover（PCのみ）
 *   .our-work-card__image-zoom            … 画像の hover（ごく小さい拡大）
 *   .our-work-card__image-float           … 画像のゆっくりした浮遊
 */
import IllustrationFrame from '../common/IllustrationFrame.vue'

defineProps({
  job: { type: Object, required: true },
  index: { type: Number, default: 0 },
})
</script>

<template>
  <article
    class="our-work-card"
    :class="`our-work-card--${job.accent || 'yellow'}`"
    :style="{ '--i': index }"
  >
    <div class="our-work-card__surface">
      <!-- カード内の装飾（文字・画像より後ろ） -->
      <span class="our-work-card__deco" aria-hidden="true">
        <span class="our-work-card__blob"></span>
        <span class="our-work-card__circle"></span>
        <span class="our-work-card__curve"></span>
        <span class="our-work-card__dots"></span>
        <span class="our-work-card__dots our-work-card__dots--bottom"></span>
      </span>

      <div class="our-work-card__image">
        <span class="our-work-card__image-zoom">
          <span class="our-work-card__image-float">
            <IllustrationFrame :src="job.image" :alt="job.imageAlt" :ratio="null" />
          </span>
        </span>
      </div>

      <div class="our-work-card__content">
        <h3 class="our-work-card__title">{{ job.title }}</h3>
        <p class="our-work-card__description">{{ job.description }}</p>
        <ul class="our-work-card__tags" aria-label="使用するスキル">
          <li v-for="tag in job.tags" :key="tag" class="our-work-card__tag">{{ tag }}</li>
        </ul>
      </div>
    </div>
  </article>
</template>

<style scoped>
/* ================= テーマカラー（Yellow / Mint / Pink） ================= */
.our-work-card--yellow {
  --ow-bg:
    radial-gradient(circle at 20% 15%, rgba(255, 218, 95, 0.18), transparent 35%),
    linear-gradient(145deg, rgba(255, 255, 255, 0.98), rgba(255, 248, 220, 0.88));
  --ow-deco: #ffc94d;     /* 装飾の色 */
  --ow-tag-bg: #ffedb8;   /* タグの背景 */
  --ow-border: rgba(255, 214, 120, 0.3);
}
.our-work-card--green {
  --ow-bg:
    radial-gradient(circle at 80% 15%, rgba(100, 230, 190, 0.16), transparent 35%),
    linear-gradient(145deg, rgba(255, 255, 255, 0.98), rgba(226, 250, 240, 0.9));
  --ow-deco: #5fd6a6;
  --ow-tag-bg: #d2f3e2;
  --ow-border: rgba(110, 220, 175, 0.3);
}
.our-work-card--pink {
  --ow-bg:
    radial-gradient(circle at 80% 15%, rgba(255, 160, 190, 0.17), transparent 35%),
    linear-gradient(145deg, rgba(255, 255, 255, 0.98), rgba(255, 232, 239, 0.9));
  --ow-deco: #ff9dbb;
  --ow-tag-bg: #ffdce7;
  --ow-border: rgba(255, 170, 195, 0.3);
}

/* ================= 浮遊（カード全体）＋ 影の呼吸 ================= */
.our-work-card {
  --float-y: 7px;
  --radius: 30px;
  position: relative;
  height: 100%;
  border-radius: var(--radius);
  animation: our-work-card-float 7s ease-in-out calc(var(--i, 0) * 0.7s) infinite;
}
/* 浮いた時だけ少し広がる影（浮遊と同じ周期） */
.our-work-card::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  box-shadow: 0 26px 52px rgba(29, 47, 78, 0.09);
  opacity: 0;
  animation: our-work-card-shadow 7s ease-in-out calc(var(--i, 0) * 0.7s) infinite;
  pointer-events: none;
}
@keyframes our-work-card-float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(calc(var(--float-y) * -1)); }
}
@keyframes our-work-card-shadow {
  0%, 100% { opacity: 0; }
  50% { opacity: 1; }
}

/* ================= カード本体 ================= */
.our-work-card__surface {
  position: relative;
  display: flex;
  flex-direction: column;
  height: 100%;
  overflow: hidden;
  background: var(--ow-bg);
  border: 1px solid var(--ow-border);
  border-radius: var(--radius);
  box-shadow:
    0 16px 40px rgba(29, 47, 78, 0.07),
    0 4px 12px rgba(29, 47, 78, 0.04),
    inset 0 1px 0 rgba(255, 255, 255, 0.9);
  transition:
    transform 0.45s cubic-bezier(0.2, 0.8, 0.2, 1),
    box-shadow 0.45s cubic-bezier(0.2, 0.8, 0.2, 1);
}

/* ---------- 装飾（よく見ると存在する程度。文字・画像より後ろ） ---------- */
.our-work-card__deco {
  position: absolute;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  transition: translate 0.45s cubic-bezier(0.2, 0.8, 0.2, 1);
}
.our-work-card__deco > * {
  position: absolute;
}
/* 淡い Blob（右下） */
.our-work-card__blob {
  right: -70px;
  bottom: -70px;
  width: 190px;
  height: 150px;
  border-radius: 58% 42% 50% 50% / 55% 60% 40% 45%;
  background: var(--ow-deco);
  opacity: 0.16;
  animation: our-work-blob 11s ease-in-out infinite;
}
/* 小さな丸（右上） */
.our-work-card__circle {
  top: 22px;
  right: 26px;
  width: 34px;
  aspect-ratio: 1;
  border-radius: 50%;
  background: var(--ow-deco);
  opacity: 0.22;
  animation: our-work-drift 12s ease-in-out infinite;
}
/* 細い曲線（画像の右側を囲むように） */
.our-work-card__curve {
  top: 40px;
  right: -60px;
  width: 200px;
  aspect-ratio: 1;
  border: 1.5px solid var(--ow-deco);
  border-color: var(--ow-deco) transparent transparent transparent;
  border-radius: 50%;
  opacity: 0.25;
  rotate: 35deg;
  animation: our-work-curve 14s ease-in-out infinite;
}
/* 小さなドット（左上 / 右下） */
.our-work-card__dots {
  top: 26px;
  left: 24px;
  width: 44px;
  height: 36px;
  background: radial-gradient(circle, var(--ow-deco) 1.6px, transparent 2.2px) 0 0 / 11px 11px;
  opacity: 0.25;
  animation: our-work-drift 10s ease-in-out -3s infinite;
}
.our-work-card__dots--bottom {
  top: auto;
  left: auto;
  right: 18px;
  bottom: 20px;
  width: 36px;
  height: 36px;
  opacity: 0.2;
  animation-delay: -6s;
}
/* テーマごとに少しだけ配置を変える（参考デザインに合わせて） */
.our-work-card--green .our-work-card__blob {
  right: -80px;
  bottom: 38%;
  width: 150px;
  height: 170px;
}
.our-work-card--green .our-work-card__circle {
  right: auto;
  left: 30%;
  top: 14px;
  width: 22px;
}
.our-work-card--pink .our-work-card__circle {
  top: 60px;
  right: 18px;
  width: 44px;
}
.our-work-card--pink .our-work-card__curve {
  right: auto;
  left: -70px;
  rotate: -40deg;
}

@keyframes our-work-blob {
  0%, 100% { scale: 1; translate: 0 0; }
  50% { scale: 1.03; translate: -4px -3px; }
}
@keyframes our-work-drift {
  0%, 100% { translate: 0 0; rotate: 0deg; }
  50% { translate: 4px -5px; rotate: 3deg; }
}
@keyframes our-work-curve {
  0%, 100% { translate: 0 0; }
  50% { translate: -5px 4px; }
}

/* ---------- 画像：カードの上側 約半分（固定比率 3:2・中央・contain） ---------- */
.our-work-card__image {
  position: relative;
  z-index: 1;
  width: 100%;
  aspect-ratio: 3 / 2;
}
.our-work-card__image-zoom {
  position: absolute;
  inset: 20px 22px 6px;
  transition: transform 0.45s cubic-bezier(0.2, 0.8, 0.2, 1);
}
.our-work-card__image-float {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  animation: our-work-image-float 6s ease-in-out calc(var(--i, 0) * -2s) infinite;
}
@keyframes our-work-image-float {
  0%, 100% { transform: translateY(0) scale(1); }
  50% { transform: translateY(-5px) scale(1.008); }
}

/* ---------- 文字：タイトル → 説明 → タグ ---------- */
.our-work-card__content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  flex: 1;
  padding: 20px 22px 28px;
}
.our-work-card__title {
  color: var(--color-navy);
  font-size: clamp(1.3125rem, 1rem + 0.45vw, 1.75rem);
  font-weight: 700;
  letter-spacing: 0.03em;
  line-height: 1.35;
}
.our-work-card__description {
  margin-top: 12px;
  color: var(--color-navy-soft);
  font-size: 0.9375rem;
  font-weight: 400;
  line-height: 1.85;
}
/* タグは各カードの下端に揃える */
.our-work-card__tags {
  display: flex;
  flex-wrap: wrap;
  gap: 7px;
  margin-top: auto;
  padding-top: 22px;
}
.our-work-card__tag {
  padding: 6px 12px;
  border-radius: 12px;
  background: var(--ow-tag-bg);
  color: #274b82;
  font-size: 0.8125rem;
  font-weight: 600;
  line-height: 1.4;
  box-shadow: inset 0 0 0 1px rgba(255, 255, 255, 0.7); /* 背景の装飾と重なっても形が分かるように */
}

/* ---------- PC（マウス操作）の hover ---------- */
@media (hover: hover) and (pointer: fine) {
  .our-work-card:hover .our-work-card__surface {
    transform: translateY(-10px);
    box-shadow:
      0 26px 56px rgba(29, 47, 78, 0.11),
      0 8px 18px rgba(29, 47, 78, 0.05),
      inset 0 1px 0 rgba(255, 255, 255, 0.9);
  }
  .our-work-card:hover .our-work-card__image-zoom {
    transform: scale(1.02);
  }
  .our-work-card:hover .our-work-card__deco {
    translate: 3px -3px;
  }
}

/* ---------- SP：浮遊を弱く、装飾を減らす ---------- */
@media (max-width: 767px) {
  .our-work-card {
    --float-y: 3px;
    --radius: 26px;
  }
  .our-work-card__content {
    padding: 18px 22px 24px;
  }
  .our-work-card__curve,
  .our-work-card__dots--bottom {
    display: none;
  }
}

@media (prefers-reduced-motion: reduce) {
  .our-work-card,
  .our-work-card::before,
  .our-work-card__image-float,
  .our-work-card__deco > * {
    animation: none;
  }
  .our-work-card__surface,
  .our-work-card__image-zoom,
  .our-work-card__deco {
    transition: none;
  }
  .our-work-card:hover .our-work-card__surface,
  .our-work-card:hover .our-work-card__image-zoom {
    transform: none;
  }
  .our-work-card:hover .our-work-card__deco {
    translate: none;
  }
}
</style>
