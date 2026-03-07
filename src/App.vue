<script setup lang="ts">
type NavItem = {
  label: string
  target: string
}

type Stat = {
  value: string
  label: string
}

type NarrativeCard = {
  title: string
  body: string
}

type FigureCard = {
  title: string
  copy: string
  src: string
  alt: string
}

type PipelineStep = {
  step: string
  title: string
  body: string
}

type BenchmarkResult = {
  name: string
  group: string
  score: number
  note: string
  tone: 'human' | 'leader' | 'open' | 'vision' | 'audio' | 'text' | 'muted'
}

type OpenEndedMetric = {
  label: string
  gemini: string
  qwen: string
}

const navigation: NavItem[] = [
  { label: 'Overview', target: '#overview' },
  { label: 'Coverage', target: '#coverage' },
  { label: 'Construction', target: '#construction' },
  { label: 'Results', target: '#results' },
  { label: 'Abstract', target: '#abstract' },
]

const heroStats: Stat[] = [
  { value: '5,000', label: 'QA pairs' },
  { value: '2,628', label: 'web videos' },
  { value: '656s', label: 'avg. duration' },
  { value: '10', label: 'domains' },
  { value: '13', label: 'skills' },
  { value: '20+', label: 'models tested' },
]

const narrativeCards: NarrativeCard[] = [
  {
    title: 'True omni-modal dependency',
    body: 'Questions are explicitly filtered so they cannot be solved from audio or vision alone. A 20% manual audit reports 100% answer correctness and 100% strict audio-visual dependency.',
  },
  {
    title: 'Long-horizon reasoning',
    body: 'MMOU focuses on long and complex real-world videos, with clips spanning 30 seconds to 84 minutes and answer evidence intentionally distributed across the entire timeline.',
  },
  {
    title: 'Expert-built challenge design',
    body: 'Eleven trained annotators create open-ended questions first, then the benchmark adds nine hard distractors per item to produce 10-option multiple-choice evaluation.',
  },
]

const designSignals: NarrativeCard[] = [
  {
    title: 'Diverse but grounded',
    body: 'The benchmark spans academic lectures, sports, travel, pranks, film, music, news, animation, gaming, and daily-life videos without collapsing into synthetic or short-clip data.',
  },
  {
    title: 'Multi-skill composition',
    body: 'Each question averages three distinct skills, forcing models to combine temporal reasoning, referential grounding, context, inference, counting, and more in one pass.',
  },
  {
    title: 'Built to break shortcuts',
    body: 'Answer evidence appears late, distractors are semantically plausible, and text-only or unimodal baselines still fail to close the gap to human performance.',
  },
]

const figureCards: FigureCard[] = [
  {
    title: 'Domain distribution',
    copy: 'MMOU covers 10 major domains with especially strong representation in academic lectures, sports, travel, daily life, pranks, and film.',
    src: '/images/mmou-domain-distribution.png',
    alt: 'Donut chart showing the domain distribution of videos in MMOU.',
  },
  {
    title: 'Skill distribution',
    copy: 'Needle-in-the-haystack, referential grounding, temporal understanding, and sequential reasoning dominate the benchmark, but all 13 skills appear in the final set.',
    src: '/images/mmou-skill-distribution.png',
    alt: 'Donut chart showing the distribution of skill categories in MMOU.',
  },
  {
    title: 'Answer evidence spread',
    copy: 'The relative position of correct evidence covers the full video timeline, preventing models from overfitting to front-loaded cues.',
    src: '/images/mmou-answer-position.png',
    alt: 'Heatmap showing where correct answer evidence appears across the video timeline.',
  },
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

const pipelineSteps: PipelineStep[] = [
  {
    step: '01',
    title: 'Skill and task curation',
    body: 'The benchmark starts from a 13-skill taxonomy designed around genuine joint audio-visual reasoning rather than single-modality perception.',
  },
  {
    step: '02',
    title: 'Domain selection',
    body: 'Ten major categories and 36 fine-grained subcategories are chosen to expose models to distinct real-world contexts and reasoning patterns.',
  },
  {
    step: '03',
    title: 'Source video collection',
    body: '2,628 long-form videos are gathered from public web sources with a focus on authentic audio conditions, temporal structure, and natural scenes.',
  },
  {
    step: '04',
    title: 'Expert question generation',
    body: 'Annotators watch each video in full, write open-ended QA pairs, and tag timestamps plus all relevant skill labels for the supporting evidence.',
  },
  {
    step: '05',
    title: 'Distractor generation',
    body: 'Nine hard distractors are synthesized per question, mixing semantically plausible and out-of-context options to suppress guessing heuristics.',
  },
  {
    step: '06',
    title: 'Quality control',
    body: 'A separate review pass removes trivial, ambiguous, weakly grounded, or poorly timestamped examples before finalization.',
  },
  {
    step: '07',
    title: 'Benchmark release',
    body: 'The final MMOU release contains 5,000 rigorously curated questions, metadata, evaluation logic, and the full benchmark package described in the paper.',
  },
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

const openEndedMetrics: OpenEndedMetric[] = [
  { label: 'Correctness', gemini: '3.41', qwen: '2.15' },
  { label: 'Completeness', gemini: '3.49', qwen: '2.02' },
  { label: 'Faithfulness', gemini: '3.89', qwen: '3.78' },
  { label: 'Clarity', gemini: '4.80', qwen: '4.62' },
  { label: 'Overall', gemini: '3.74', qwen: '2.81' },
]

const openPaper = () => {
  if (typeof window !== 'undefined') {
    window.open('/downloads/mmou-paper.pdf', '_blank', 'noopener,noreferrer')
  }
}

const jumpTo = (target: string) => {
  if (typeof document === 'undefined') {
    return
  }

  document.querySelector(target)?.scrollIntoView({
    behavior: 'smooth',
    block: 'start',
  })
}

const scoreStyle = (score: number) => ({ width: `${score}%` })
</script>

<template>
  <UApp>
    <div class="site-shell">
      <header class="sticky top-0 z-50 px-4 pt-4 sm:px-6 lg:px-8">
        <div class="chrome-panel mx-auto flex max-w-7xl items-center justify-between gap-4 rounded-full px-4 py-3 sm:px-6">
          <button type="button" class="flex items-center gap-3 text-left" @click="jumpTo('#hero')">
            <img
              src="/images/mmou-logo.png"
              alt="MMOU logo"
              class="h-11 w-11 rounded-full border border-white/55 bg-white/90 object-cover p-1 shadow-sm"
            >
            <div class="hidden sm:block">
              <p class="text-xs uppercase tracking-[0.32em] text-[var(--muted)]">MMOU</p>
              <p class="text-sm font-semibold text-[var(--ink)]">Massive Multi-Task Omni Understanding</p>
            </div>
          </button>

          <nav class="hidden items-center gap-2 lg:flex">
            <button
              v-for="item in navigation"
              :key="item.target"
              type="button"
              class="nav-link"
              @click="jumpTo(item.target)"
            >
              {{ item.label }}
            </button>
          </nav>

          <div class="flex items-center gap-2">
            <UButton
              color="neutral"
              variant="ghost"
              class="rounded-full px-4 text-sm"
              @click="jumpTo('#results')"
            >
              Results
            </UButton>
            <UButton color="neutral" class="rounded-full px-5 text-sm" @click="openPaper">
              Read Paper
            </UButton>
          </div>
        </div>
      </header>

      <main>
        <section id="hero" class="mx-auto max-w-7xl px-6 pb-16 pt-10 sm:px-8 lg:px-12 lg:pb-24 lg:pt-16">
          <div class="grid items-center gap-10 lg:grid-cols-[0.98fr_1.02fr] lg:gap-14">
            <div class="reveal space-y-8">
              <div class="space-y-4">
                <UBadge
                  color="neutral"
                  variant="soft"
                  size="lg"
                  class="rounded-full border border-white/60 bg-white/80 px-4 py-1 text-[0.72rem] uppercase tracking-[0.28em] text-[var(--muted)]"
                >
                  New benchmark paper
                </UBadge>
                <h1 class="display-font max-w-4xl text-5xl leading-[0.92] tracking-[-0.04em] text-[var(--ink)] sm:text-6xl lg:text-7xl">
                  Long, complex, real-world video reasoning still breaks frontier multimodal models.
                </h1>
                <p class="max-w-2xl text-base/8 text-[var(--muted)] sm:text-lg/8">
                  MMOU is a large-scale benchmark for audio-visual reasoning over long web videos. It tests whether models can jointly use sound, vision, context, and temporal evidence instead of relying on short clips or shallow shortcuts.
                </p>
              </div>

              <div class="flex flex-wrap items-center gap-3">
                <UButton size="xl" color="neutral" class="rounded-full px-6" @click="openPaper">
                  Open the paper
                </UButton>
                <UButton
                  size="xl"
                  color="neutral"
                  variant="outline"
                  class="rounded-full border-[var(--border-strong)] bg-white/60 px-6 text-[var(--ink)]"
                  @click="jumpTo('#overview')"
                >
                  Explore the benchmark
                </UButton>
              </div>

              <div class="grid gap-3 sm:grid-cols-2 xl:grid-cols-3">
                <div
                  v-for="stat in heroStats"
                  :key="stat.label"
                  class="metric-pill reveal rounded-[24px] px-4 py-4"
                >
                  <p class="text-2xl font-extrabold tracking-[-0.04em] text-[var(--ink)]">{{ stat.value }}</p>
                  <p class="mt-1 text-sm font-medium text-[var(--muted)]">{{ stat.label }}</p>
                </div>
              </div>
            </div>

            <div class="reveal relative lg:min-h-[42rem]">
              <div class="hero-aurora absolute inset-x-[12%] top-8 h-48 rounded-full blur-3xl" />
              <div class="hero-ring absolute -left-8 top-16 h-24 w-24 rounded-full border border-[var(--border-strong)]" />

              <UCard class="surface-card media-card relative overflow-hidden rounded-[36px] p-4 sm:p-5">
                <img
                  src="/images/mmou-hero.png"
                  alt="Hero figure showing a long video strip, a benchmark question, and the performance gap between models on MMOU."
                  class="h-full w-full rounded-[28px] object-cover"
                >
              </UCard>

              <div class="floating-panel left-0 top-6 hidden max-w-xs lg:block">
                <p class="text-[0.68rem] font-bold uppercase tracking-[0.3em] text-[var(--accent-strong)]">
                  Why it is hard
                </p>
                <p class="mt-3 text-lg font-semibold text-[var(--ink)]">
                  One answer can require temporal reasoning, referential grounding, and audio perception at once.
                </p>
              </div>

              <div class="floating-panel floating-panel-alt -bottom-4 right-0 hidden w-[18rem] lg:block">
                <p class="text-[0.68rem] font-bold uppercase tracking-[0.3em] text-[var(--accent-strong)]">
                  Headline gap
                </p>
                <div class="mt-4 space-y-3">
                  <div
                    v-for="item in results.slice(0, 3)"
                    :key="item.name"
                    class="rounded-2xl border border-[var(--border)] bg-white/72 px-4 py-3"
                  >
                    <div class="mb-2 flex items-center justify-between gap-3">
                      <p class="text-sm font-semibold text-[var(--ink)]">{{ item.name }}</p>
                      <p class="text-sm font-bold text-[var(--ink)]">{{ item.score }}%</p>
                    </div>
                    <div class="score-track">
                      <div :class="['score-fill', `score-fill--${item.tone}`]" :style="scoreStyle(item.score)" />
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </section>

        <section id="overview" class="mx-auto max-w-7xl px-6 py-8 sm:px-8 lg:px-12 lg:py-12">
          <div class="section-heading reveal">
            <UBadge color="neutral" variant="soft" class="section-badge">Overview</UBadge>
            <h2 class="display-font section-title">A benchmark designed to punish unimodal shortcuts.</h2>
            <p class="section-copy">
              MMOU measures long-form omni-modal reasoning under realistic conditions: late evidence, entangled audio and visual cues, multi-skill composition, and hard distractors that discourage guessable multiple choice behavior.
            </p>
          </div>

          <div class="mt-10 grid gap-6 lg:grid-cols-3">
            <UCard
              v-for="card in narrativeCards"
              :key="card.title"
              class="surface-card rounded-[30px] p-2"
            >
              <div class="space-y-4 px-4 py-5">
                <p class="text-[0.72rem] font-bold uppercase tracking-[0.28em] text-[var(--accent-strong)]">
                  Signal
                </p>
                <h3 class="text-2xl font-semibold tracking-[-0.03em] text-[var(--ink)]">{{ card.title }}</h3>
                <p class="text-sm/7 text-[var(--muted)]">{{ card.body }}</p>
              </div>
            </UCard>
          </div>

          <div class="mt-8 grid gap-6 lg:grid-cols-[0.96fr_1.04fr]">
            <UCard class="surface-card rounded-[30px] p-2">
              <div class="space-y-5 px-4 py-5">
                <p class="text-[0.72rem] font-bold uppercase tracking-[0.28em] text-[var(--accent-strong)]">
                  Core design
                </p>
                <h3 class="text-3xl font-semibold tracking-[-0.04em] text-[var(--ink)]">
                  MMOU forces models to ground answers in full-video evidence.
                </h3>
                <div class="grid gap-4 sm:grid-cols-3">
                  <div
                    v-for="signal in designSignals"
                    :key="signal.title"
                    class="rounded-[22px] border border-[var(--border)] bg-white/72 px-4 py-4"
                  >
                    <p class="text-sm font-semibold text-[var(--ink)]">{{ signal.title }}</p>
                    <p class="mt-2 text-sm/7 text-[var(--muted)]">{{ signal.body }}</p>
                  </div>
                </div>
              </div>
            </UCard>

            <UCard class="surface-card rounded-[30px] p-2">
              <div class="space-y-5 px-4 py-5">
                <p class="text-[0.72rem] font-bold uppercase tracking-[0.28em] text-[var(--accent-strong)]">
                  Main claim
                </p>
                <p class="text-lg/8 text-[var(--ink)]">
                  Even the best reported closed-source model reaches only
                  <span class="font-bold text-[var(--accent-strong)]">57.5%</span> accuracy, while the strongest open-source omni model reaches
                  <span class="font-bold text-[var(--accent-strong)]">36.3%</span>.
                  Human accuracy is
                  <span class="font-bold text-[var(--accent-strong)]">84.3%</span>.
                </p>
                <p class="text-sm/7 text-[var(--muted)]">
                  That gap persists across domain shifts, long durations, multi-skill questions, and open-ended evaluation. MMOU is not a benchmark for isolated perception - it is a benchmark for synchronized audio-visual reasoning over long-form video.
                </p>
                <div class="rounded-[24px] border border-[var(--border)] bg-[var(--surface-soft)] px-5 py-4">
                  <p class="text-xs uppercase tracking-[0.26em] text-[var(--muted)]">Dataset snapshot</p>
                  <div class="mt-4 grid gap-3 sm:grid-cols-2">
                    <div class="rounded-2xl bg-white/75 px-4 py-3">
                      <p class="text-sm font-semibold text-[var(--ink)]">30s to 84 min videos</p>
                      <p class="mt-1 text-sm text-[var(--muted)]">Average duration: 656.0 seconds.</p>
                    </div>
                    <div class="rounded-2xl bg-white/75 px-4 py-3">
                      <p class="text-sm font-semibold text-[var(--ink)]">10 answer options</p>
                      <p class="mt-1 text-sm text-[var(--muted)]">Each question includes nine hard distractors.</p>
                    </div>
                  </div>
                </div>
              </div>
            </UCard>
          </div>
        </section>

        <section id="coverage" class="mx-auto max-w-7xl px-6 py-14 sm:px-8 lg:px-12 lg:py-20">
          <div class="section-heading reveal">
            <UBadge color="neutral" variant="soft" class="section-badge">Coverage</UBadge>
            <h2 class="display-font section-title">Breadth in domains, depth in skills, spread across time.</h2>
            <p class="section-copy">
              The benchmark combines broad web coverage with carefully controlled annotation choices so the evidence can appear early, late, or anywhere in between.
            </p>
          </div>

          <div class="mt-10 grid gap-6 lg:grid-cols-3">
            <UCard
              v-for="figure in figureCards"
              :key="figure.title"
              class="surface-card media-card rounded-[30px] p-2"
            >
              <figure class="space-y-4 px-4 py-5">
                <img :src="figure.src" :alt="figure.alt" loading="lazy" class="w-full rounded-[24px] bg-white object-cover">
                <figcaption>
                  <p class="text-xl font-semibold tracking-[-0.03em] text-[var(--ink)]">{{ figure.title }}</p>
                  <p class="mt-2 text-sm/7 text-[var(--muted)]">{{ figure.copy }}</p>
                </figcaption>
              </figure>
            </UCard>
          </div>

          <div class="mt-8 grid gap-6 lg:grid-cols-[0.8fr_1.2fr]">
            <UCard class="surface-card rounded-[30px] p-2">
              <div class="space-y-5 px-4 py-5">
                <p class="text-[0.72rem] font-bold uppercase tracking-[0.28em] text-[var(--accent-strong)]">
                  Domains
                </p>
                <h3 class="text-2xl font-semibold tracking-[-0.03em] text-[var(--ink)]">
                  Real web video, not narrow benchmark clips.
                </h3>
                <div class="chip-grid">
                  <span v-for="domain in domains" :key="domain" class="info-chip">
                    {{ domain }}
                  </span>
                </div>
              </div>
            </UCard>

            <UCard class="surface-card rounded-[30px] p-2">
              <div class="space-y-5 px-4 py-5">
                <p class="text-[0.72rem] font-bold uppercase tracking-[0.28em] text-[var(--accent-strong)]">
                  Skills
                </p>
                <h3 class="text-2xl font-semibold tracking-[-0.03em] text-[var(--ink)]">
                  13 skill types, with multiple skills active in a single question.
                </h3>
                <div class="chip-grid chip-grid-wide">
                  <span v-for="skill in skills" :key="skill" class="info-chip info-chip-wide">
                    {{ skill }}
                  </span>
                </div>
              </div>
            </UCard>
          </div>
        </section>

        <section id="construction" class="mx-auto max-w-7xl px-6 py-14 sm:px-8 lg:px-12 lg:py-20">
          <div class="section-heading reveal">
            <UBadge color="neutral" variant="soft" class="section-badge">Construction</UBadge>
            <h2 class="display-font section-title">From skill taxonomy to expert review, every stage is explicit.</h2>
            <p class="section-copy">
              MMOU is built through a structured pipeline that starts with reasoning skills and ends with strict quality control. The benchmark is deliberately engineered rather than scraped and labeled at scale.
            </p>
          </div>

          <div class="mt-10 grid gap-6 lg:grid-cols-[1.1fr_0.9fr]">
            <UCard class="surface-card media-card rounded-[34px] p-3">
              <figure class="space-y-4 px-2 py-2">
                <img
                  src="/images/mmou-pipeline.png"
                  alt="Pipeline figure illustrating MMOU construction from skill curation to final benchmark release."
                  loading="lazy"
                  class="w-full rounded-[26px] bg-white object-cover"
                >
                <figcaption class="px-2 pb-2 text-sm/7 text-[var(--muted)]">
                  The paper pipeline moves from skill curation and domain selection to source-video collection, expert question writing, distractor generation, review, and final release packaging.
                </figcaption>
              </figure>
            </UCard>

            <div class="grid gap-4">
              <UCard
                v-for="step in pipelineSteps"
                :key="step.step"
                class="surface-card rounded-[28px] p-2"
              >
                <div class="flex gap-4 px-4 py-4">
                  <div class="step-badge">{{ step.step }}</div>
                  <div class="space-y-2">
                    <p class="text-lg font-semibold tracking-[-0.02em] text-[var(--ink)]">{{ step.title }}</p>
                    <p class="text-sm/7 text-[var(--muted)]">{{ step.body }}</p>
                  </div>
                </div>
              </UCard>
            </div>
          </div>
        </section>

        <section id="results" class="mx-auto max-w-7xl px-6 py-14 sm:px-8 lg:px-12 lg:py-20">
          <div class="results-shell">
            <div class="section-heading section-heading-dark reveal">
              <UBadge color="neutral" variant="soft" class="section-badge section-badge-dark">Results</UBadge>
              <h2 class="display-font section-title text-white">
                MMOU exposes a large gap between pattern matching and real multi-modal understanding.
              </h2>
              <p class="section-copy text-white/72">
                Skill-level breakdowns, late-evidence analysis, and open-ended scoring all point to the same conclusion: present-day models still struggle with robust audio-visual reasoning over long, messy video.
              </p>
            </div>

            <div class="mt-10 grid gap-6 lg:grid-cols-[0.95fr_1.05fr]">
              <UCard class="surface-card-dark rounded-[32px] p-2">
                <div class="space-y-5 px-4 py-5 sm:px-5">
                  <p class="text-[0.72rem] font-bold uppercase tracking-[0.28em] text-[var(--accent-soft)]">
                    Scoreboard
                  </p>
                  <h3 class="text-3xl font-semibold tracking-[-0.04em] text-white">
                    Best reported accuracy still stops far below human performance.
                  </h3>

                  <div class="space-y-4">
                    <div
                      v-for="item in results"
                      :key="item.name"
                      class="rounded-[24px] border border-white/10 bg-white/[0.04] px-4 py-4"
                    >
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

              <UCard class="surface-card-dark media-card rounded-[32px] p-3">
                <figure class="space-y-4 px-2 py-2">
                  <img
                    src="/images/mmou-radar.png"
                    alt="Radar chart comparing model performance across MMOU skills."
                    loading="lazy"
                    class="w-full rounded-[26px] bg-white object-cover"
                  >
                  <figcaption class="px-2 pb-2">
                    <p class="text-xl font-semibold text-white">Skill-wise performance remains uneven.</p>
                    <p class="mt-2 text-sm/7 text-white/62">
                      The paper highlights persistent weaknesses in counting, temporal understanding, and needle-in-the-haystack reasoning even for frontier omni models.
                    </p>
                  </figcaption>
                </figure>
              </UCard>
            </div>

            <div class="mt-6 grid gap-6 lg:grid-cols-[1.02fr_0.98fr]">
              <UCard class="surface-card-dark media-card rounded-[32px] p-3">
                <figure class="space-y-4 px-2 py-2">
                  <img
                    src="/images/mmou-late-evidence.png"
                    alt="Line chart showing that model accuracy drops as answer evidence appears later in long videos."
                    loading="lazy"
                    class="w-full rounded-[26px] bg-white object-cover"
                  >
                  <figcaption class="px-2 pb-2">
                    <p class="text-xl font-semibold text-white">Late evidence hurts every model.</p>
                    <p class="mt-2 text-sm/7 text-white/62">
                      Accuracy steadily degrades as relevant evidence appears later in long videos, revealing a long-horizon context retention problem that current models do not solve.
                    </p>
                  </figcaption>
                </figure>
              </UCard>

              <UCard class="surface-card-dark rounded-[32px] p-3">
                <div class="space-y-5 px-2 py-2">
                  <figure class="space-y-4">
                    <img
                      src="/images/mmou-open-ended.png"
                      alt="Heatmap showing open-ended evaluation scores by skill on MMOU."
                      loading="lazy"
                      class="w-full rounded-[26px] bg-white object-cover"
                    >
                    <figcaption class="px-2">
                      <p class="text-xl font-semibold text-white">Open-ended evaluation stays hard.</p>
                      <p class="mt-2 text-sm/7 text-white/62">
                        Gemini 2.5 Pro leads the open-ended setting too, but correctness and completeness remain much weaker than fluency and clarity.
                      </p>
                    </figcaption>
                  </figure>

                  <div class="grid gap-3 sm:grid-cols-2">
                    <div
                      v-for="metric in openEndedMetrics"
                      :key="metric.label"
                      class="rounded-[22px] border border-white/10 bg-white/[0.04] px-4 py-4"
                    >
                      <p class="text-sm font-semibold text-white">{{ metric.label }}</p>
                      <div class="mt-3 flex items-end justify-between gap-6">
                        <div>
                          <p class="text-xs uppercase tracking-[0.24em] text-white/42">Gemini 2.5 Pro</p>
                          <p class="mt-1 text-2xl font-bold tracking-[-0.03em] text-white">{{ metric.gemini }}</p>
                        </div>
                        <div class="text-right">
                          <p class="text-xs uppercase tracking-[0.24em] text-white/42">Qwen3-Omni-30B</p>
                          <p class="mt-1 text-2xl font-bold tracking-[-0.03em] text-white">{{ metric.qwen }}</p>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </UCard>
            </div>
          </div>
        </section>

        <section id="abstract" class="mx-auto max-w-7xl px-6 pb-20 pt-8 sm:px-8 lg:px-12 lg:pb-28">
          <div class="grid gap-6 lg:grid-cols-[0.92fr_1.08fr]">
            <UCard class="surface-card rounded-[32px] p-2">
              <div class="space-y-5 px-4 py-5 sm:px-5">
                <p class="text-[0.72rem] font-bold uppercase tracking-[0.28em] text-[var(--accent-strong)]">
                  Abstract
                </p>
                <h2 class="display-font text-4xl tracking-[-0.04em] text-[var(--ink)]">
                  MMOU is a diagnostic benchmark for the next generation of long-video multimodal models.
                </h2>
                <p class="text-base/8 text-[var(--muted)]">
                  The benchmark contains 5,000 carefully curated questions over 2,628 long-form web videos with tightly coupled audio and visual content. It spans 13 core reasoning skills, diverse real-world domains, and intentionally difficult answer structures. Across more than 20 evaluated models, the paper shows that current systems still struggle to jointly reason over synchronized audio, visual detail, and long temporal context.
                </p>
                <div class="flex flex-wrap gap-3">
                  <UButton color="neutral" class="rounded-full px-5" @click="openPaper">
                    Read the full PDF
                  </UButton>
                  <UButton
                    color="neutral"
                    variant="outline"
                    class="rounded-full border-[var(--border-strong)] bg-white/65 px-5 text-[var(--ink)]"
                    @click="jumpTo('#construction')"
                  >
                    See the pipeline
                  </UButton>
                </div>
              </div>
            </UCard>

            <UCard class="surface-card rounded-[32px] p-2">
              <div class="space-y-5 px-4 py-5 sm:px-5">
                <p class="text-[0.72rem] font-bold uppercase tracking-[0.28em] text-[var(--accent-strong)]">
                  Release scope
                </p>
                <div class="grid gap-4 sm:grid-cols-2">
                  <div class="rounded-[24px] border border-[var(--border)] bg-white/72 px-4 py-4">
                    <p class="text-lg font-semibold tracking-[-0.02em] text-[var(--ink)]">What the paper contributes</p>
                    <p class="mt-2 text-sm/7 text-[var(--muted)]">
                      A new long-video omni-modal benchmark, extensive baseline evaluation, skill-wise analysis, temporal-position analysis, and open-ended scoring.
                    </p>
                  </div>
                  <div class="rounded-[24px] border border-[var(--border)] bg-white/72 px-4 py-4">
                    <p class="text-lg font-semibold tracking-[-0.02em] text-[var(--ink)]">What the release will include</p>
                    <p class="mt-2 text-sm/7 text-[var(--muted)]">
                      The paper states that the benchmark data, evaluation code, and associated metadata will be open-sourced as part of the MMOU release.
                    </p>
                  </div>
                </div>
                <div class="rounded-[26px] border border-[var(--border)] bg-[var(--surface-soft)] px-5 py-5">
                  <p class="text-xs uppercase tracking-[0.26em] text-[var(--muted)]">Fast summary</p>
                  <p class="mt-3 text-lg/8 text-[var(--ink)]">
                    MMOU is not another short-clip leaderboard. It is a benchmark for models that need to watch, listen, localize, compare, remember, and infer over long, noisy, real-world video.
                  </p>
                </div>
              </div>
            </UCard>
          </div>
        </section>
      </main>

      <footer class="mx-auto max-w-7xl px-6 pb-10 sm:px-8 lg:px-12">
        <div class="footer-panel flex flex-col gap-5 rounded-[28px] px-6 py-6 sm:flex-row sm:items-center sm:justify-between">
          <div>
            <p class="text-xs uppercase tracking-[0.28em] text-[var(--muted)]">MMOU paper site</p>
            <p class="mt-2 text-sm text-[var(--muted)]">
              Built from the local paper sources and figures in this repository.
            </p>
          </div>
          <div class="flex flex-wrap items-center gap-3">
            <button type="button" class="nav-link nav-link-strong" @click="jumpTo('#hero')">
              Back to top
            </button>
            <UButton color="neutral" class="rounded-full px-5" @click="openPaper">
              Paper PDF
            </UButton>
          </div>
        </div>
      </footer>
    </div>
  </UApp>
</template>
