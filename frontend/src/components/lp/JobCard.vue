<script setup>
import IllustrationFrame from '../common/IllustrationFrame.vue'

defineProps({
  job: { type: Object, required: true },
  index: { type: Number, default: 0 },
})

// ブロブの呼吸の周期（3枚が同時に動かないようにずらす）
const BREATH = ['9s', '11s', '10s']
</script>

<template>
  <article
    class="job-card"
    :class="`accent-${job.accent || 'yellow'}`"
    :style="{ '--breath': BREATH[index % BREATH.length], '--breath-delay': `${index * -2.5}s` }"
  >
    <div class="job-card__visual">
      <span class="job-card__blob" aria-hidden="true"></span>
      <!-- 画像は共通の表示枠（背景透過PNG推奨）。枠いっぱいに広げて表示 -->
      <span class="job-card__frame">
        <IllustrationFrame :src="job.image" :alt="job.imageAlt" :ratio="null" />
      </span>
    </div>
    <div class="job-card__body">
      <h3 class="job-card__title">{{ job.title }}</h3>
      <p class="job-card__text">{{ job.description }}</p>
      <ul class="job-card__tags" aria-label="使用するスキル">
        <li v-for="tag in job.tags" :key="tag" class="job-card__tag">{{ tag }}</li>
      </ul>
    </div>
  </article>
</template>

<style scoped>
.job-card {
  --card-shadow: 0 16px 36px rgba(31, 54, 105, 0.07), 0 4px 12px rgba(31, 54, 105, 0.04);
  --card-shadow-hover: 0 22px 46px rgba(31, 54, 105, 0.1), 0 6px 16px rgba(31, 54, 105, 0.05);
  display: flex;
  flex-direction: column;
  height: 100%;
  overflow: hidden;
  background: #fffefb;
  border: 1px solid rgba(31, 54, 105, 0.08);
  border-radius: 22px;
  box-shadow: var(--card-shadow);
  transition: transform 0.35s cubic-bezier(0.22, 0.61, 0.36, 1), box-shadow 0.35s ease;
}

/* ---------- 上部：淡い Organic Blob（CSS）＋ イラスト（PNG） ---------- */
/*
 * 画像エリアはすべてのカード共通の固定比率（3:2）。
 * カード幅が同じなら画像エリアの高さも同じになるため、
 * どんな画像に差し替えてもタイトル位置・カードの高さは揃ったまま。
 * 推奨画像：背景透過PNG・余白が少ない・横長（約3:2）。比率が違っても枠内に収まります。
 */
.job-card__visual {
  position: relative;
  width: 100%;
  aspect-ratio: 3 / 2;
}
/* 画像を入れる枠：絶対配置なので、画像の大きさ・比率がレイアウトに影響しない */
.job-card__frame {
  position: absolute;
  inset: 16px 18px 4px;
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}
.job-card__blob {
  position: absolute;
  inset: 14px 16px 2px;
  border-radius: 45% 55% 48% 52% / 52% 44% 56% 48%;
  background: var(--accent-pale);
  opacity: 0.9;
  animation: job-blob-breathe var(--breath, 10s) ease-in-out var(--breath-delay, 0s) infinite;
}
/* イラストの表示は共通の IllustrationFrame（contain・色の加工なし）。hover 用の動きだけここで指定 */
.job-card__frame {
  transition: transform 0.35s cubic-bezier(0.22, 0.61, 0.36, 1);
}
@keyframes job-blob-breathe {
  0%, 100% { scale: 1; opacity: 0.9; }
  50% { scale: 1.015; opacity: 1; }
}

/* ---------- 下部：白い情報エリア ---------- */
.job-card__body {
  display: flex;
  flex-direction: column;
  flex: 1;
  padding: 18px 24px 24px;
}
.job-card__title {
  color: var(--color-navy);
  font-size: clamp(1.125rem, 1rem + 0.35vw, 1.3125rem);
  font-weight: 800;
  letter-spacing: 0.03em;
  line-height: 1.4;
}
.job-card__text {
  margin-top: 10px;
  color: var(--color-text);
  font-size: 0.875rem;
  line-height: 1.8;
}
/* タグは各カードの下端に揃える */
.job-card__tags {
  display: flex;
  flex-wrap: wrap;
  gap: 7px;
  margin-top: auto;
  padding-top: 18px;
}
.job-card__tag {
  padding: 4px 9px;
  border-radius: 7px;
  background: #eef5ff;
  color: #26467c;
  font-size: 0.75rem;
  font-weight: 600;
  line-height: 1.5;
}

/* ---------- PC（マウス操作）の hover：少し浮く ---------- */
@media (hover: hover) and (pointer: fine) {
  .job-card:hover {
    transform: translateY(-6px);
    box-shadow: var(--card-shadow-hover);
  }
  .job-card:hover .job-card__frame {
    transform: translateY(-3px) scale(1.01);
  }
}

@media (max-width: 767px) {
  .job-card__body {
    padding: 16px 20px 22px;
  }
}

@media (prefers-reduced-motion: reduce) {
  .job-card,
  .job-card__frame {
    transition: none;
  }
  .job-card:hover,
  .job-card:hover .job-card__frame {
    transform: none;
  }
  .job-card__blob {
    animation: none;
  }
}
</style>
