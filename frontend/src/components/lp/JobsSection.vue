<script setup>
import { lpContent } from '../../data/lpContent.js'
import { img } from '../../utils/image.js'
import SectionHeading from './SectionHeading.vue'
import JobCard from './JobCard.vue'

const { jobsSection: section, jobs } = lpContent
</script>

<template>
  <section id="jobs" class="jobs lp-section lp-section--cream" aria-labelledby="jobs-title">
    <span class="lp-blob lp-blob--green jobs__blob-1" aria-hidden="true"></span>
    <span class="lp-blob lp-blob--blue jobs__blob-2" aria-hidden="true"></span>
    <img class="lp-deco jobs__leaf" :src="img('common/deco-leaf.svg')" alt="" aria-hidden="true" />
    <img class="lp-deco jobs__sparkle" :src="img('common/deco-sparkle.svg')" alt="" aria-hidden="true" />

    <div class="lp-container">
      <SectionHeading
        id="jobs-title"
        :number="section.number"
        :label="section.label"
        :title="section.title"
        :lead="section.lead"
        align="center"
      />
      <ul class="jobs__list">
        <li v-for="(job, index) in jobs" :key="job.title" v-reveal="{ delay: 100, i: index }">
          <JobCard :job="job" :index="index" />
        </li>
      </ul>
    </div>
  </section>
</template>

<style scoped>
.jobs__list {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: clamp(20px, 2.2vw, 30px);
  margin-top: var(--space-xl);
}
.jobs__blob-1 {
  width: 360px;
  height: 320px;
  left: -140px;
  top: 60px;
}
.jobs__blob-2 {
  width: 300px;
  height: 280px;
  right: -100px;
  bottom: 40px;
}
.jobs__leaf {
  width: 56px;
  right: 8%;
  top: 72px;
  transform: rotate(18deg);
}
.jobs__sparkle {
  width: 44px;
  left: 30%;
  top: 60px;
}

/* Tablet：2列＋1列（3枚目は中央に） */
@media (max-width: 1024px) {
  .jobs__list {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
  .jobs__list > li:last-child:nth-child(odd) {
    grid-column: 1 / -1;
    justify-self: center;
    width: calc(50% - clamp(20px, 2.2vw, 30px) / 2);
  }
}
@media (max-width: 767px) {
  .jobs__list {
    grid-template-columns: 1fr;
    max-width: 440px;
    margin-inline: auto;
  }
  .jobs__list > li:last-child:nth-child(odd) {
    width: 100%;
  }
  .jobs__sparkle {
    left: 8%;
  }
}
</style>
