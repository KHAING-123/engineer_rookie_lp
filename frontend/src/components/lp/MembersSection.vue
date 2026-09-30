<script setup>
import { lpContent } from '../../data/lpContent.js'
import { img } from '../../utils/image.js'
import MemberCard from './MemberCard.vue'

const { membersSection: section, members } = lpContent

// 見出しの \n は「幅が足りないときだけ改行する位置」として扱う（PCでは1行表示）
const titleParts = section.title.split('\n')
const labels = {
  nameSuffix: section.nameSuffix,
  ageUnit: section.ageUnit,
  previousPrefix: section.previousPrefix,
}
</script>

<template>
  <section id="members" class="members lp-section" aria-labelledby="members-title">
    <!-- 背景装飾（すべて飾り） -->
    <div class="members__decor" aria-hidden="true">
      <span class="members__circle members__circle--yellow-l"></span>
      <span class="members__circle members__circle--pink-l"></span>
      <span class="members__circle members__circle--green-r"></span>
      <span class="members__circle members__circle--yellow-r"></span>
      <img class="members__deco members__deco--dots" :src="img('common/deco-dots.svg')" alt="" />
      <img class="members__deco members__deco--leaf-l" :src="img('common/deco-leaf.svg')" alt="" />
      <img class="members__deco members__deco--leaf-r" :src="img('common/deco-leaf-pair.svg')" alt="" />
      <span class="members__spark members__spark--r"></span>
    </div>

    <div class="lp-container members__grid">
      <!-- 左：01 + 見出し + 説明 + 手書き風メッセージ -->
      <div class="members__intro">
        <div class="members__head">
          <span v-reveal="{ variant: 'fade' }" class="members__number" aria-hidden="true">{{ section.number }}</span>
          <div class="members__head-text">
            <span v-if="section.label" v-reveal class="members__label" aria-hidden="true">{{ section.label }}</span>
            <h2 v-reveal id="members-title" class="members__title">
              <template v-for="(part, i) in titleParts" :key="i"><wbr v-if="i > 0" />{{ part }}</template>
            </h2>
            <p v-if="section.lead" v-reveal="{ delay: 100 }" class="members__lead pre-line">{{ section.lead }}</p>
          </div>
        </div>

        <p v-if="section.handwritten" v-reveal="{ variant: 'fade', delay: 350 }" class="members__handwritten pre-line">{{ section.handwritten }}</p>
      </div>

      <!-- 右：メンバー3名 + 補助メッセージ -->
      <div class="members__people">
        <ul class="members__list">
          <li v-for="(member, index) in members" :key="member.name" v-reveal="{ delay: 100, i: index }">
            <MemberCard :member="member" :labels="labels" />
          </li>
        </ul>

        <p v-if="section.sideNote" v-reveal="{ delay: 450 }" class="members__side-note">
          <span class="pre-line">{{ section.sideNote }}</span>
        </p>
      </div>
    </div>
  </section>
</template>

<style scoped>
/* ---------- セクション全体 ---------- */
.members {
  --members-bg-top: #fffdf7;
  --members-bg-bottom: #fff8e8;
  --members-hand-blue: #3a6db0;
  /* Note の浮遊量（SPで弱める） */
  --note-a-y: 10px;
  --note-b-y: 8px;
  --note-x: 3px;
  --note-rot: 1deg;
  background: linear-gradient(180deg, var(--members-bg-top) 0%, var(--members-bg-bottom) 100%);
}
.members__grid {
  display: grid;
  grid-template-columns: minmax(0, 4.4fr) minmax(0, 5.6fr);
  gap: clamp(24px, 3vw, 48px);
  align-items: start;
}

/* ---------- 左：見出し ---------- */
.members__intro {
  position: relative;
  display: flex;
  flex-direction: column;
}
.members__head {
  display: flex;
  align-items: flex-start;
  gap: clamp(14px, 1.6vw, 22px);
}
.members__number {
  position: relative;
  flex-shrink: 0;
  color: var(--color-coral);
  font-family: var(--font-number);
  font-size: clamp(4rem, 3rem + 3vw, 6rem);
  font-weight: 900;
  line-height: 0.85;
  letter-spacing: -0.02em;
  transform: scaleY(1.08);
  transform-origin: top;
}
/* 01 の右上の小さなアクセント線 */
.members__number::after {
  content: '';
  position: absolute;
  top: -10px;
  right: -12px;
  width: 14px;
  height: 14px;
  border-top: 3px solid var(--color-coral);
  border-right: 3px solid var(--color-coral);
  border-radius: 0 10px 0 0;
  opacity: 0.55;
  transform: rotate(10deg);
}
.members__head-text {
  min-width: 0;
  padding-top: 4px;
}
.members__label {
  display: block;
  margin-bottom: 4px;
  color: var(--color-coral);
  font-size: 0.6875rem;
  font-weight: 700;
  letter-spacing: 0.2em;
  opacity: 0.8;
}
.members__title {
  color: var(--color-navy);
  font-size: clamp(1.5rem, 1.2rem + 0.6vw, 1.875rem);
  font-weight: 900;
  letter-spacing: 0.03em;
  line-height: 1.4;
  word-break: keep-all; /* \n の位置（<wbr>）でだけ改行する */
  overflow-wrap: anywhere;
}
.members__lead {
  max-width: 30em;
  margin-top: 16px;
  color: var(--color-navy-soft);
  font-size: 0.9375rem;
  line-height: 1.95;
}

/* 左下の手書き風メッセージ（右カラムへつながる位置に） */
.members__handwritten {
  align-self: flex-end;
  margin-top: clamp(28px, 4vw, 56px);
  margin-right: clamp(0px, 3vw, 40px);
  color: var(--members-hand-blue);
  font-family: var(--font-heading);
  font-size: 1.0625rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  line-height: 1.9;
  transform: rotate(-9deg);
  /* Note B：文字＋下線ごと、紙が少し浮いているように */
  filter: drop-shadow(0 6px 10px rgba(31, 54, 105, 0.1));
  animation: note-float-b 7.4s ease-in-out -2.5s infinite;
}
.members__handwritten::after {
  content: '';
  display: block;
  width: 70%;
  height: 8px;
  margin-top: 2px;
  border-bottom: 2px solid currentColor;
  border-radius: 0 0 50% 50%;
  opacity: 0.35;
}

/* ---------- 右：メンバー ---------- */
.members__people {
  display: grid;
  grid-template-columns: minmax(0, 1fr) auto;
  gap: 12px;
  align-items: center;
}
.members__list {
  display: grid;
  gap: 14px;
}
/* PCだけ少しずらして自然な配置感に（1・2人目を右へ） */
.members__list > li:nth-child(3n + 1),
.members__list > li:nth-child(3n + 2) {
  padding-left: clamp(0px, 3.2vw, 48px);
}

/* 右端：ピンクの丸の補助メッセージ */
.members__side-note {
  position: relative;
  display: grid;
  place-items: center;
  width: clamp(118px, 10.5vw, 150px);
  aspect-ratio: 1;
  margin-top: -40px;
  border-radius: 55% 45% 50% 50% / 50% 55% 45% 50%;
  background: var(--color-pink-pale);
  color: var(--color-navy);
  font-family: var(--font-heading);
  font-size: clamp(0.8125rem, 0.5rem + 0.45vw, 0.9375rem);
  font-weight: 700;
  letter-spacing: 0.08em;
  line-height: 1.8;
  text-align: center;
  /* Note A：ピンクの丸ごと浮かせる（柔らかい影＋ごく薄いピンクのGlow） */
  box-shadow: 0 12px 26px rgba(31, 54, 105, 0.07), 0 4px 10px rgba(31, 54, 105, 0.04),
    0 0 28px rgba(255, 159, 165, 0.22);
  animation: note-float-a 6.2s ease-in-out -1s infinite;
}
.members__side-note > span {
  display: block;
  transform: rotate(-8deg);
}
/* 丸の左上の小さな線 */
.members__side-note::before {
  content: '';
  position: absolute;
  top: 4px;
  left: 2px;
  width: 18px;
  height: 18px;
  border-top: 2px solid var(--color-coral);
  border-left: 2px solid var(--color-coral);
  border-radius: 12px 0 0 0;
  opacity: 0.6;
}

/* ---------- Note の浮遊（translate / rotate は既存の transform に重ねて適用） ---------- */
@keyframes note-float-a {
  0%,
  100% {
    translate: 0 0;
    rotate: calc(var(--note-rot) * -1);
  }
  50% {
    translate: 0 calc(var(--note-a-y) * -1);
    rotate: var(--note-rot);
  }
}
@keyframes note-float-b {
  0%,
  100% {
    translate: 0 0;
    rotate: var(--note-rot);
  }
  40% {
    translate: var(--note-x) calc(var(--note-b-y) * -1);
    rotate: calc(var(--note-rot) * -1);
  }
  75% {
    translate: calc(var(--note-x) * -0.7) calc(var(--note-b-y) * -0.45);
    rotate: 0deg;
  }
}
@media (prefers-reduced-motion: reduce) {
  .members__side-note,
  .members__handwritten {
    animation: none;
  }
}

/* ---------- 背景装飾 ---------- */
.members__decor {
  position: absolute;
  inset: 0;
  z-index: 0;
  pointer-events: none;
}
.members__decor > * {
  position: absolute;
  pointer-events: none;
  user-select: none;
}
.members__circle {
  border-radius: 50%;
}
.members__circle--yellow-l {
  width: 380px;
  height: 380px;
  left: -140px;
  bottom: -120px;
  background: var(--color-yellow-pale);
  opacity: 0.8;
}
.members__circle--pink-l {
  width: 280px;
  height: 240px;
  left: 60px;
  bottom: -130px;
  border-radius: 58% 42% 55% 45% / 48% 58% 42% 52%;
  background: var(--color-pink-pale);
  opacity: 0.7;
}
.members__circle--green-r {
  width: 120px;
  height: 120px;
  right: 4%;
  top: -30px;
  background: var(--color-green-pale);
}
.members__circle--yellow-r {
  width: 200px;
  height: 200px;
  right: -60px;
  bottom: 40px;
  background: var(--color-yellow-pale);
  opacity: 0.85;
}
.members__deco--dots {
  width: 80px;
  top: 36px;
  left: 40%;
  opacity: 0.6;
}
.members__deco--leaf-l {
  width: 40px;
  left: 22%;
  bottom: 48px;
  transform: rotate(-30deg);
  opacity: 0.7;
}
.members__deco--leaf-r {
  width: 64px;
  right: 7%;
  bottom: 110px;
  opacity: 0.75;
}
.members__spark--r {
  right: 3%;
  top: 34%;
  width: 22px;
  height: 22px;
  border-top: 2px solid var(--color-yellow-dark);
  border-right: 2px solid var(--color-yellow-dark);
  border-radius: 0 14px 0 0;
  opacity: 0.6;
}

/* ---------- Tablet（〜1024px）：縦方向に並べる ---------- */
@media (max-width: 1024px) {
  .members__grid {
    grid-template-columns: 1fr;
    gap: 32px;
  }
  .members__handwritten {
    align-self: flex-start;
    margin: 24px 0 0 clamp(80px, 12vw, 120px);
  }
  .members__people {
    grid-template-columns: 1fr;
    justify-items: center;
    gap: 24px;
  }
  .members__list {
    width: 100%;
    max-width: 600px;
  }
  .members__side-note {
    width: 150px;
    margin-top: 0;
    font-size: 0.9375rem;
  }
}

/* ---------- SP（〜767px） ---------- */
@media (max-width: 767px) {
  /* Note の浮遊をPCより弱く */
  .members {
    --note-a-y: 5px;
    --note-b-y: 4px;
    --note-x: 2px;
    --note-rot: 0.5deg;
  }
  /* 01＋タイトルを1行目に、説明文は全幅で下に */
  .members__head {
    display: grid;
    grid-template-columns: auto minmax(0, 1fr);
    column-gap: 12px;
    align-items: end;
  }
  .members__head-text {
    display: contents;
  }
  .members__number {
    grid-row: 1 / span 2;
  }
  .members__label,
  .members__title {
    grid-column: 2;
  }
  .members__lead {
    grid-column: 1 / -1;
    max-width: none;
  }
  .members__number {
    font-size: 3.5rem;
  }
  .members__lead {
    font-size: var(--fs-sm);
  }
  .members__handwritten {
    margin-left: auto;
    margin-right: 8%;
    font-size: 1rem;
  }
  .members__list > li:nth-child(3n + 1),
  .members__list > li:nth-child(3n + 2) {
    padding-left: 0;
  }
  .members__deco--dots {
    left: auto;
    right: 16px;
    top: 16px;
  }
  .members__circle--green-r {
    width: 80px;
    height: 80px;
  }
}
</style>
