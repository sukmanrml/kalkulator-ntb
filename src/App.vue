<script setup lang="ts">
import { Calculator, ShieldCheck } from '@lucide/vue';
import { Badge } from '@/components/ui/badge';
import InputPanel from '@/components/InputPanel.vue';
import ResultPanel from '@/components/ResultPanel.vue';
import ThemeToggle from '@/components/ThemeToggle.vue';
import { useNtbCalculator } from '@/composables/useNtbCalculator';

const { input, result, issues, text, started, categoryItems, kbliItems, ntbData, setKbli, setCategory, loadSample, reset } = useNtbCalculator();
</script>

<template>
  <div class="min-h-screen">
    <header class="no-print border-b bg-card">
      <div class="mx-auto flex max-w-6xl items-center gap-3 px-4 py-4">
        <div class="grid size-10 shrink-0 place-items-center rounded-xl bg-primary text-primary-foreground"><Calculator class="size-5" aria-hidden="true" /></div>
        <div class="min-w-0 flex-1">
          <h1 class="text-xl font-semibold tracking-tight">Kalkulator NTB</h1>
          <p class="text-sm text-muted-foreground">Periksa kewajaran Nilai Tambah Bruto, upah, dan komponen biaya usaha.</p>
        </div>
        <Badge variant="secondary" class="hidden sm:inline-flex"><ShieldCheck aria-hidden="true" /> Data tetap di browser Anda</Badge>
        <ThemeToggle />
      </div>
    </header>

    <main class="mx-auto grid max-w-6xl items-start gap-5 px-4 py-6 lg:grid-cols-[minmax(0,5fr)_minmax(0,6fr)]">
      <InputPanel v-model="input" :category-items="categoryItems" :kbli-items="kbliItems" @category="setCategory" @kbli="setKbli" @sample="loadSample" @reset="reset" />
      <div class="lg:sticky lg:top-4">
        <ResultPanel :result="result" :input="input" :issues="issues" :text="text" :started="started" :data="ntbData" />
      </div>
    </main>

    <footer class="no-print mx-auto max-w-6xl px-4 pb-10 text-sm text-muted-foreground">
      <p>Batas kewajaran berasal dari berkas Kalkulator NTB_SharedV1.1. Alat ini membantu pemeriksaan awal, bukan keputusan akhir. Angka tidak dikirim atau disimpan di mana pun.</p>
    </footer>
  </div>
</template>
