<script setup lang="ts">
import { ref } from 'vue'

const clicks = ref(0)

const stack = [
  'Vite 7',
  'Vue 3 + TypeScript',
  'Tailwind CSS 4',
  'Nuxt UI 4',
]

const commands = [
  { label: 'npm run dev', description: 'Start the local development server.' },
  { label: 'npm run build', description: 'Type-check and build for production.' },
  { label: 'npm run preview', description: 'Preview the production build locally.' },
]
</script>

<template>
  <UApp>
    <main class="min-h-screen px-6 py-16 text-default">
      <div class="mx-auto flex min-h-screen max-w-6xl items-center">
        <div class="grid w-full gap-6 lg:grid-cols-[1.1fr_0.9fr]">
          <section class="space-y-6">
            <UBadge
              color="neutral"
              variant="subtle"
              size="lg"
              class="rounded-full px-4 py-1 text-[0.7rem] uppercase tracking-[0.24em]"
            >
              Clean Starter
            </UBadge>

            <div class="space-y-4">
              <h1 class="max-w-3xl text-4xl font-semibold tracking-tight sm:text-5xl lg:text-6xl">
                Vite, Vue, Tailwind, and Nuxt UI are wired up and ready.
              </h1>
              <p class="max-w-2xl text-base/7 text-muted sm:text-lg/8">
                The previous survey-specific application code and assets have been removed. Use this screen as the minimal baseline for your next build.
              </p>
            </div>

            <div class="flex flex-wrap items-center gap-3">
              <UButton size="lg" color="neutral" class="rounded-full px-5" @click="clicks += 1">
                Nuxt UI button works
              </UButton>
              <div class="inline-flex items-center rounded-full border border-white/70 bg-white/80 px-4 py-2 text-sm font-medium text-slate-700 shadow-sm shadow-slate-900/5 backdrop-blur-sm dark:border-white/10 dark:bg-white/5 dark:text-slate-100">
                Clicks: {{ clicks }}
              </div>
            </div>
          </section>

          <div class="grid gap-4">
            <UCard class="rounded-[28px] border-white/70 bg-white/78 shadow-xl shadow-slate-900/10 backdrop-blur dark:border-white/10 dark:bg-slate-950/60 dark:shadow-black/30">
              <template #header>
                <div>
                  <p class="text-sm font-medium text-muted">Stack</p>
                  <h2 class="text-xl font-semibold text-highlighted">Included in this setup</h2>
                </div>
              </template>

              <ul class="space-y-3">
                <li
                  v-for="item in stack"
                  :key="item"
                  class="flex items-center justify-between gap-4 rounded-2xl border border-slate-200/70 bg-slate-50/80 px-4 py-3 dark:border-white/10 dark:bg-white/5"
                >
                  <span class="font-medium">{{ item }}</span>
                  <UBadge color="success" variant="soft">Ready</UBadge>
                </li>
              </ul>
            </UCard>

            <UCard class="rounded-[28px] border-white/70 bg-white/78 shadow-xl shadow-slate-900/10 backdrop-blur dark:border-white/10 dark:bg-slate-950/60 dark:shadow-black/30">
              <template #header>
                <div>
                  <p class="text-sm font-medium text-muted">Commands</p>
                  <h2 class="text-xl font-semibold text-highlighted">Common tasks</h2>
                </div>
              </template>

              <div class="space-y-3">
                <div
                  v-for="command in commands"
                  :key="command.label"
                  class="rounded-2xl border border-slate-200/70 bg-slate-50/80 px-4 py-3 dark:border-white/10 dark:bg-white/5"
                >
                  <code class="text-sm font-semibold text-highlighted">{{ command.label }}</code>
                  <p class="mt-1 text-sm text-muted">{{ command.description }}</p>
                </div>
              </div>
            </UCard>
          </div>
        </div>
      </div>
    </main>
  </UApp>
</template>
