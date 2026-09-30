<script setup>
/**
 * メンバー1人分：丸背景の人物画像 ＋ 横長プロフィールカード
 * 文字・画像はすべて lpContent.js の members / membersSection から受け取ります。
 */
import { img } from '../../utils/image.js'

defineProps({
  member: { type: Object, required: true },
  labels: { type: Object, default: () => ({}) }, // nameSuffix / ageUnit / previousPrefix
})
</script>

<template>
  <article class="member" :class="`accent-${member.accent || 'blue'}`">
    <div class="member__avatar">
      <img :src="img(member.image)" :alt="member.imageAlt" width="240" height="240" loading="lazy" />
    </div>

    <div class="member__card">
      <h3 class="member__name">
        {{ member.name }}{{ labels.nameSuffix }}<span class="member__sep" aria-hidden="true"> / </span><span class="visually-hidden">、</span>{{ member.age }}{{ labels.ageUnit }}
      </h3>
      <p class="member__career">
        <span class="member__prev">{{ labels.previousPrefix }}{{ member.previousJob }}</span>
        <span class="member__arrow" aria-hidden="true">→</span><span class="visually-hidden">から</span>
        <strong class="member__now">{{ member.currentRole }}</strong>
      </p>
      <p class="member__comment">{{ member.comment }}</p>
    </div>
  </article>
</template>

<style scoped>
.member {
  --avatar-size: clamp(116px, 10.5vw, 150px);
  --avatar-overlap: clamp(22px, 2vw, 30px);
  /* Premium Shadow：ネイビー系の薄い影＋ベージュ系の暖かい影 */
  --card-shadow: 0 10px 30px rgba(31, 54, 105, 0.07), 0 3px 10px rgba(31, 54, 105, 0.04),
    0 18px 34px -20px rgba(196, 140, 96, 0.22);
  --card-shadow-hover: 0 16px 40px rgba(31, 54, 105, 0.1), 0 5px 14px rgba(31, 54, 105, 0.05),
    0 24px 40px -22px rgba(196, 140, 96, 0.26);
  display: flex;
  align-items: center;
  /* 人物＋プロフィールBoxを1つのMember Itemとして動かす */
  transition: transform 0.35s cubic-bezier(0.22, 0.61, 0.36, 1);
}

/* 人物：淡色の丸の上に画像（縦横比を保って丸の中に収める） */
.member__avatar {
  position: relative;
  z-index: 2;
  flex-shrink: 0;
  width: var(--avatar-size);
  height: var(--avatar-size);
  border-radius: 50%;
  background: radial-gradient(circle at 35% 30%, #ffffff 0%, var(--accent-pale) 55%);
  /* 白いリングは維持し、プロフィールBoxより弱い柔らかな影 */
  box-shadow: 0 0 0 5px rgba(255, 255, 255, 0.9), 0 8px 24px rgba(31, 54, 105, 0.06),
    0 14px 26px -16px rgba(196, 140, 96, 0.18);
  overflow: hidden;
}
.member__avatar img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  object-position: center bottom;
}

/* 横長プロフィールカード（人物が左側に少し重なる） */
.member__card {
  position: relative;
  z-index: 1;
  flex: 1;
  min-width: 0;
  max-width: 400px;
  margin-left: calc(var(--avatar-overlap) * -1);
  padding: 16px 22px 16px calc(var(--avatar-overlap) + 18px);
  background: rgba(255, 255, 255, 0.96);
  border: 1px solid #efe1c0;
  border-radius: 16px;
  box-shadow: var(--card-shadow);
  transition: box-shadow 0.35s ease;
}

/* PC（マウス操作）のみ：hover で約5pxだけ浮き、影が少し深くなる */
@media (hover: hover) and (pointer: fine) {
  .member:hover {
    transform: translateY(-5px);
  }
  .member:hover .member__card {
    box-shadow: var(--card-shadow-hover);
  }
}
@media (prefers-reduced-motion: reduce) {
  .member {
    transition: none;
  }
  .member:hover {
    transform: none;
  }
}
.member__name {
  color: var(--color-navy);
  font-size: var(--fs-base);
  font-weight: 900;
  line-height: 1.5;
}
.member__sep {
  color: var(--color-text-muted);
  font-weight: 700;
}
.member__career {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 2px 8px;
  margin-top: 2px;
  font-size: var(--fs-xs);
  line-height: 1.6;
}
.member__prev {
  color: var(--color-text-muted);
  font-weight: 500;
}
.member__arrow {
  color: var(--color-coral);
  font-weight: 700;
}
.member__now {
  color: var(--color-navy);
  font-weight: 700;
}
.member__comment {
  position: relative;
  margin-top: 10px;
  padding-top: 10px;
  border-top: 1px dashed #efe3c4;
  color: var(--color-navy);
  font-size: var(--fs-sm);
  font-weight: 500;
  line-height: 1.75;
}
.member__comment::before {
  content: '「';
}
.member__comment::after {
  content: '」';
}

@media (max-width: 600px) {
  .member {
    --avatar-size: 92px;
    --avatar-overlap: 18px;
    align-items: flex-start;
  }
  .member__card {
    padding: 14px 16px 14px calc(var(--avatar-overlap) + 14px);
    margin-top: 12px;
  }
}
/* とても狭い画面：人物 → カードの縦並び */
@media (max-width: 359px) {
  .member {
    flex-direction: column;
    align-items: center;
  }
  .member__card {
    width: 100%;
    margin-left: 0;
    margin-top: -20px;
    padding: 28px 16px 14px;
  }
}
</style>
