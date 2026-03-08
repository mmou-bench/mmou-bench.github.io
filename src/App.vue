<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

type NavItem = {
  label: string
  target: string
}

type Stat = {
  value: string
  label: string
}

type BenchmarkResult = {
  name: string
  group: string
  score: number
  note: string
  tone: 'human' | 'leader' | 'open' | 'vision' | 'audio' | 'text' | 'muted'
}

const navigation: NavItem[] = [
  { label: 'Abstract', target: '#abstract' },
  { label: 'Figure', target: '#figure-1' },
  { label: 'Benchmark', target: '#benchmark' },
  { label: 'Leaderboard', target: '#leaderboard' },
]

const colorModeSelectUi = {
  base: 'mode-select-trigger',
  value: 'mode-select-value',
  placeholder: 'mode-select-placeholder',
  leadingIcon: 'mode-select-icon',
  trailingIcon: 'mode-select-icon',
  itemLeadingIcon: 'mode-select-icon',
  content: 'mode-select-content',
  item: 'mode-select-item',
} as const

let previousScrollRestoration: ScrollRestoration | null = null

const activeSection = ref(navigation[0]?.target ?? '#abstract')

const benchmarkStats: Stat[] = [
  { value: '5,000', label: 'QA pairs' },
  { value: '2,628', label: 'web videos' },
  { value: '656s', label: 'avg. duration' },
  { value: '10', label: 'domains' },
  { value: '13', label: 'skills' },
  { value: '20+', label: 'models tested' },
]

const domains = [
  'Academic Lectures',
  'Sports',
  'Travel',
  'Video Games',
  'Daily Life',
  'Pranks',
  'Film',
  'Animation',
  'Music',
  'News',
]

const skills = [
  'Needle-in-the-haystack',
  'Referential grounding',
  'Temporal understanding',
  'Sequential reasoning',
  'Context understanding',
  'Subscene understanding',
  'Inference',
  'Counting',
  'Comparative reasoning',
  'Audio-visual stitching',
  'Tackling spurious correlations',
  'Object interaction reasoning',
  'General holistic reasoning',
]


const results: BenchmarkResult[] = [
  {
    name: 'Human',
    group: 'Reference upper bound',
    score: 84.3,
    note: 'Expert annotators remain well ahead of every evaluated model.',
    tone: 'human',
  },
  {
    name: 'Gemini 2.5 Pro',
    group: 'Best closed-source omni model',
    score: 57.5,
    note: 'The strongest overall baseline still misses more than 4 in 10 questions.',
    tone: 'leader',
  },
  {
    name: 'Qwen3-Omni-30B-Instruct',
    group: 'Best open-source omni model',
    score: 36.3,
    note: 'Open models lag far behind the strongest proprietary baseline.',
    tone: 'open',
  },
  {
    name: 'Qwen3-VL-32B-Instruct',
    group: 'Vision-only baseline',
    score: 38.3,
    note: 'Strong visual reasoning is still insufficient when audio carries critical evidence.',
    tone: 'vision',
  },
  {
    name: 'Qwen3-Omni-30B (audio only)',
    group: 'Audio-only baseline',
    score: 35.6,
    note: 'Audio alone also collapses on questions that need grounded visual context.',
    tone: 'audio',
  },
  {
    name: 'GPT-5.2',
    group: 'Text-only baseline',
    score: 40.7,
    note: 'Language priors help, but they do not replace temporally grounded perception.',
    tone: 'text',
  },
  {
    name: 'Audio Flamingo 3',
    group: 'Audio-only reference',
    score: 15.6,
    note: 'Pure audio reasoning fails on most of MMOU.',
    tone: 'muted',
  },
]


const openPaper = () => {
  if (typeof window !== 'undefined') {
    window.open('/mmou.pdf', '_blank', 'noopener,noreferrer')
  }
}

const updateActiveSection = () => {
  if (typeof window === 'undefined' || typeof document === 'undefined') {
    return
  }

  const probe = window.innerHeight * 0.32
  let currentTarget = navigation[0]?.target ?? '#abstract'

  for (const item of navigation) {
    const section = document.querySelector<HTMLElement>(item.target)

    if (!section) {
      continue
    }

    if (section.getBoundingClientRect().top <= probe) {
      currentTarget = item.target
    }
  }

  activeSection.value = currentTarget
}

const jumpTo = (target: string) => {
  if (typeof document === 'undefined') {
    return
  }

  activeSection.value = target

  document.querySelector(target)?.scrollIntoView({
    behavior: 'smooth',
    block: 'start',
  })
}

const scoreStyle = (score: number) => ({ width: `${score}%` })

const resetInitialScroll = () => {
  if (typeof window === 'undefined') {
    return
  }

  if ('scrollRestoration' in window.history) {
    previousScrollRestoration = window.history.scrollRestoration
    window.history.scrollRestoration = 'manual'
  }

  if (window.location.hash) {
    return
  }

  requestAnimationFrame(() => {
    window.scrollTo({ top: 0, left: 0 })
    updateActiveSection()
  })
}

onMounted(() => {
  resetInitialScroll()
  updateActiveSection()
  window.addEventListener('scroll', updateActiveSection, { passive: true })
  window.addEventListener('resize', updateActiveSection)
})

onBeforeUnmount(() => {
  if (typeof window !== 'undefined' && previousScrollRestoration) {
    window.history.scrollRestoration = previousScrollRestoration
  }

  window.removeEventListener('scroll', updateActiveSection)
  window.removeEventListener('resize', updateActiveSection)
})
</script>

<template>
  <UApp>
    <div class="site-shell">
      <div class="ambient-backdrop" aria-hidden="true">
        <div class="ambient-backdrop__layer ambient-backdrop__layer--orbits"></div>
        <div class="ambient-backdrop__layer ambient-backdrop__layer--constellation"></div>
        <div class="ambient-backdrop__layer ambient-backdrop__layer--waveform"></div>
        <div class="ambient-backdrop__layer ambient-backdrop__layer--timeline"></div>
        <div class="ambient-backdrop__layer ambient-backdrop__layer--signals"></div>
      </div>

      <header class="fixed inset-x-0 top-3 z-[60] px-4 sm:px-6 lg:px-8">
        <div
          class="chrome-panel mx-auto flex max-w-7xl items-center justify-between gap-3 rounded-full px-4 py-2 sm:px-6">
          <button type="button" class="brand-button flex items-center gap-3 text-left" @click="jumpTo('#abstract')">
            <img src="/images/mmou-logo.png" alt="MMOU logo" class="h-11 w-auto object-contain sm:h-12">
            <div class="hidden sm:block">
              <p class="text-xs uppercase tracking-[0.32em] text-[var(--muted)]">MMOU</p>
              <p class="text-sm font-semibold text-[var(--ink)]">Massive Multi-Task Omni Understanding</p>
            </div>
          </button>

          <nav class="hidden items-center gap-2 lg:flex">
            <button v-for="item in navigation" :key="item.target" type="button"
              :class="['nav-link', { 'nav-link-active': activeSection === item.target }]"
              :aria-current="activeSection === item.target ? 'page' : undefined" @click="jumpTo(item.target)">
              {{ item.label }}
            </button>
          </nav>

          <div class="flex items-center gap-2">
            <UButton color="neutral" class="button-fx shrink-0 whitespace-nowrap rounded-full px-5 text-sm"
              @click="openPaper">
              Paper PDF
            </UButton>
            <UColorModeSelect color="neutral" variant="outline" size="xs" class="mode-select w-[6.75rem] sm:w-[7.25rem]"
              :content="{ side: 'bottom', align: 'end', sideOffset: 10 }" :ui="colorModeSelectUi" />
          </div>
        </div>
      </header>

      <main class="pt-[5.75rem] sm:pt-24 lg:pt-28">
        <section id="abstract"
          class="mx-auto max-w-7xl scroll-mt-28 px-6 pb-8 pt-4 sm:px-8 sm:pt-5 lg:scroll-mt-32 lg:px-12 lg:pb-10 lg:pt-6">
          <div class="mx-auto max-w-5xl">
            <UCard class="surface-card rounded-[34px] p-2">
              <div class="space-y-5 px-4 py-5 sm:px-6 sm:py-6">
                <div class="flex items-start gap-4">
                  <img src="/images/mmou-logo.png" alt="MMOU logo" class="h-14 w-auto object-contain sm:h-16">
                  <div class="min-w-0">
                    <p class="text-[0.72rem] font-bold uppercase tracking-[0.3em] text-[var(--accent-strong)]">
                      MMOU
                    </p>
                    <h1 class="display-font mt-2 text-4xl leading-[0.96] tracking-[-0.04em] text-[var(--ink)] sm:text-5xl">
                      Massive Multi-Task Omni Understanding
                    </h1>
                  </div>
                </div>
                <p class="text-[0.72rem] font-bold uppercase tracking-[0.3em] text-[var(--accent-strong)]">
                  Abstract
                </p>
                <p class="text-base/8 text-[var(--muted)] sm:text-lg/8">
                  MMOU introduces a large-scale benchmark for long-form omni-modal reasoning over real-world web videos.
                  It contains 5,000 expert-curated question-answer pairs from 2,628 videos, covering 10 domains and 13
                  reasoning skills. Across more than 20 evaluated models, the paper shows that current systems still
                  struggle to jointly use audio, visual detail, and long-range temporal context, remaining well below
                  human performance on difficult real-world video understanding.
                </p>
                <div class="flex flex-wrap gap-3">
                  <UButton color="neutral" class="button-fx rounded-full px-5" @click="openPaper">
                    Read the paper
                  </UButton>
                  <UButton color="neutral" variant="outline"
                    class="button-fx rounded-full border-[var(--border-strong)] bg-white/65 px-5 text-[var(--ink)]"
                    @click="jumpTo('#figure-1')">
                    View Figure
                  </UButton>
                </div>
              </div>
            </UCard>
          </div>
        </section>

        <section id="figure-1"
          class="mx-auto max-w-7xl scroll-mt-28 px-6 py-8 sm:px-8 lg:scroll-mt-32 lg:px-12 lg:py-10">
          <div class="section-heading reveal">
            <UBadge color="neutral" variant="soft" class="section-badge">Figure</UBadge>
            <h2 class="display-font section-title">A single MMOU question can require audio, vision, and temporal reasoning together.</h2>
            <p class="section-copy">
              The main paper figure illustrates the benchmark format: long input video, temporally localized evidence,
              cross-modal reasoning requirements, and a clear performance gap between humans and current models.
            </p>
          </div>

          <UCard class="surface-card media-card mt-8 rounded-[34px] p-2">
            <figure class="space-y-4 px-2 py-2">
              <img src="/images/mmou-hero.png"
                alt="Main MMOU figure showing a long video example, multi-skill reasoning requirements, and the performance gap between three representative models."
                class="w-full rounded-[26px] bg-white" loading="eager">
              <figcaption class="grid gap-4 px-2 pb-2 lg:grid-cols-[1.05fr_0.95fr] lg:items-start">
                <div>
                  <p class="text-2xl font-semibold tracking-[-0.03em] text-[var(--ink)]">Example task and top-line gap</p>
                  <p class="mt-2 text-sm/7 text-[var(--muted)]">
                    This example highlights why MMOU is difficult: answering one question can require tracking events
                    over time, grounding referenced objects and people, and using audio cues that are inseparable from
                    the visual context.
                  </p>
                </div>
                <div class="grid gap-2.5 sm:grid-cols-3">
                  <div v-for="item in results.slice(0, 3)" :key="item.name"
                    class="rounded-[22px] border border-[var(--border)] bg-white/72 px-4 py-4">
                    <div class="mb-2 flex items-center justify-between gap-3">
                      <p class="text-sm font-semibold text-[var(--ink)]">{{ item.name }}</p>
                      <p class="text-sm font-bold text-[var(--ink)]">{{ item.score }}%</p>
                    </div>
                    <div class="score-track">
                      <div :class="['score-fill', `score-fill--${item.tone}`]" :style="scoreStyle(item.score)" />
                    </div>
                  </div>
                </div>
              </figcaption>
            </figure>
          </UCard>
        </section>

        <section id="benchmark"
          class="mx-auto max-w-7xl scroll-mt-28 px-6 py-8 sm:px-8 lg:scroll-mt-32 lg:px-12 lg:py-10">
          <div class="section-heading reveal">
            <UBadge color="neutral" variant="soft" class="section-badge">Benchmark</UBadge>
            <h2 class="display-font section-title">Benchmark overview</h2>
            <p class="section-copy">
              MMOU combines long-form video duration, broad domain coverage, and multi-skill question design in one
              benchmark for real audio-visual understanding.
            </p>
          </div>

          <div class="mt-8 grid gap-3 sm:grid-cols-2 xl:grid-cols-3">
            <div v-for="stat in benchmarkStats" :key="stat.label" class="metric-pill reveal rounded-[24px] px-4 py-4">
              <p class="text-2xl font-extrabold tracking-[-0.04em] text-[var(--ink)]">{{ stat.value }}</p>
              <p class="mt-1 text-sm font-medium text-[var(--muted)]">{{ stat.label }}</p>
            </div>
          </div>

          <div class="mt-4 grid gap-4 lg:grid-cols-2">
            <UCard class="surface-card media-card rounded-[30px] p-2">
              <div class="space-y-4 px-4 py-4">
                <figure class="space-y-3">
                  <img src="/images/mmou-domain-distribution.png"
                    alt="Donut chart showing the distribution of MMOU videos across major domains." loading="lazy"
                    class="w-full rounded-[24px] bg-white">
                  <figcaption>
                    <p class="text-xl font-semibold tracking-[-0.03em] text-[var(--ink)]">Domains</p>
                    <p class="mt-2 text-sm/7 text-[var(--muted)]">
                      MMOU spans academic lectures, sports, travel, games, daily-life footage, pranks, film,
                      animation, music, and news.
                    </p>
                  </figcaption>
                </figure>
                <div class="chip-grid">
                  <span v-for="domain in domains" :key="domain" class="info-chip">
                    {{ domain }}
                  </span>
                </div>
              </div>
            </UCard>

            <UCard class="surface-card media-card rounded-[30px] p-2">
              <div class="space-y-4 px-4 py-4">
                <figure class="space-y-3">
                  <img src="/images/mmou-skill-distribution.png"
                    alt="Donut chart showing the distribution of MMOU reasoning skills." loading="lazy"
                    class="w-full rounded-[24px] bg-white">
                  <figcaption>
                    <p class="text-xl font-semibold tracking-[-0.03em] text-[var(--ink)]">Skills</p>
                    <p class="mt-2 text-sm/7 text-[var(--muted)]">
                      Questions cover 13 reasoning skills, with multiple skills often active in the same example.
                    </p>
                  </figcaption>
                </figure>
                <div class="chip-grid chip-grid-wide">
                  <span v-for="skill in skills" :key="skill" class="info-chip info-chip-wide">
                    {{ skill }}
                  </span>
                </div>
              </div>
            </UCard>
          </div>
        </section>

        <section id="leaderboard"
          class="mx-auto max-w-7xl scroll-mt-28 px-6 py-8 sm:px-8 lg:scroll-mt-32 lg:px-12 lg:py-10">
          <div class="results-shell">
            <div class="section-heading section-heading-dark reveal">
              <UBadge color="neutral" variant="soft" class="section-badge section-badge-dark">Leaderboard</UBadge>
              <h2 class="display-font section-title text-white">
                Reported accuracy on MMOU
              </h2>
              <p class="section-copy text-white/72">
                Human performance remains clearly ahead of the best reported models, with the strongest closed-source
                and open-source systems still leaving a large gap on long-form omni-modal reasoning.
              </p>
            </div>

            <UCard class="surface-card-dark mt-8 rounded-[32px] p-2">
              <div class="space-y-4 px-4 py-4 sm:px-5">
                <h3 class="text-3xl font-semibold tracking-[-0.04em] text-white">
                  Best reported accuracy remains far below human performance.
                </h3>

                <div class="space-y-3">
                  <div v-for="item in results" :key="item.name"
                    class="rounded-[24px] border border-white/10 bg-white/[0.04] px-4 py-4">
                    <div class="flex items-center justify-between gap-4">
                      <div>
                        <p class="text-base font-semibold text-white">{{ item.name }}</p>
                        <p class="text-sm text-white/55">{{ item.group }}</p>
                      </div>
                      <p class="text-xl font-bold tracking-[-0.03em] text-white">{{ item.score }}%</p>
                    </div>
                    <div class="mt-3 score-track score-track-dark">
                      <div :class="['score-fill', `score-fill--${item.tone}`]" :style="scoreStyle(item.score)" />
                    </div>
                    <p class="mt-3 text-sm/7 text-white/62">{{ item.note }}</p>
                  </div>
                </div>
              </div>
            </UCard>
          </div>
        </section>
      </main>

      <footer class="mx-auto max-w-7xl px-6 pb-8 sm:px-8 lg:px-12">
        <div
          class="footer-panel flex flex-col gap-4 rounded-[28px] px-5 py-5 sm:flex-row sm:items-center sm:justify-between">
          <div>
            <p class="text-xs uppercase tracking-[0.28em] text-[var(--muted)]">MMOU</p>
            <p class="mt-2 text-sm text-[var(--muted)]">
              Massive Multi-Task Omni Understanding for long and complex real-world video reasoning.
            </p>
          </div>
          <div class="flex flex-wrap items-center gap-3">
            <UButton color="neutral" variant="outline"
              class="button-fx min-w-[8.75rem] justify-center whitespace-nowrap rounded-full border-[var(--border-strong)] bg-white/68 px-5 text-sm font-semibold text-[var(--ink)]"
              @click="jumpTo('#abstract')">
              Back to top
            </UButton>
            <UButton color="neutral"
              class="button-fx min-w-[8.75rem] justify-center whitespace-nowrap rounded-full px-5 text-sm"
              @click="openPaper">
              Paper PDF
            </UButton>
          </div>
        </div>
      </footer>
    </div>
  </UApp>
</template>
