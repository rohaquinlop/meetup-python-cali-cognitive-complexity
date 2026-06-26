<script setup lang="ts">
const props = withDefaults(defineProps<{
  a: number
  b: number
  labelA?: string
  labelB?: string
  /** 'vs' compares two programs, 'arrow' shows a refactor reduction */
  mode?: 'vs' | 'arrow'
  note?: string
  /** hero size, for statement / section slides */
  big?: boolean
}>(), { mode: 'arrow', big: false })

/** Severity ramp, the deck's signature: low cools, high burns. */
function tone(v: number) {
  if (v <= 5) return 'var(--cogc-low)'
  if (v <= 10) return 'var(--cogc-mid)'
  return 'var(--cogc-high)'
}

const reduction = props.b < props.a
  ? Math.round(((props.a - props.b) / props.a) * 100)
  : 0
</script>

<template>
  <div class="cogc-versus" :class="{ 'cogc-versus--big': big }">
    <div class="cogc-versus__row">
      <div class="cogc-cell">
        <span class="cogc-eyebrow">CogC</span>
        <span class="cogc-num" :style="{ color: tone(a) }">{{ a }}</span>
        <span v-if="labelA" class="cogc-cap">{{ labelA }}</span>
      </div>

      <div class="cogc-sep">{{ mode === 'arrow' ? '→' : 'vs' }}</div>

      <div class="cogc-cell">
        <span class="cogc-eyebrow">CogC</span>
        <span class="cogc-num" :style="{ color: tone(b) }">{{ b }}</span>
        <span v-if="labelB" class="cogc-cap">{{ labelB }}</span>
      </div>

      <div v-if="mode === 'arrow' && reduction > 0" class="cogc-delta">
        −{{ reduction }}%
      </div>
    </div>

    <div v-if="note" class="cogc-note">{{ note }}</div>
  </div>
</template>

<style scoped>
.cogc-versus {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}
.cogc-versus__row {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 22px;
}
.cogc-cell {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 56px;
}
.cogc-eyebrow {
  font-family: 'Space Grotesk', sans-serif;
  font-size: 0.6rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--text-very-muted);
  margin-bottom: 1px;
}
.cogc-num {
  font-family: 'JetBrains Mono', monospace;
  font-size: 2.7rem;
  font-weight: 600;
  line-height: 1;
  font-variant-numeric: tabular-nums;
}
.cogc-cap {
  font-size: 0.66rem;
  color: var(--text-muted);
  margin-top: 6px;
  max-width: 150px;
  text-align: center;
  line-height: 1.3;
}
.cogc-sep {
  font-family: 'Space Grotesk', sans-serif;
  font-size: 1.3rem;
  font-weight: 400;
  color: var(--accent-teal);
  align-self: flex-start;
  margin-top: 22px;
}
.cogc-delta {
  align-self: flex-start;
  margin-top: 20px;
  margin-left: 4px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.72rem;
  font-weight: 600;
  color: var(--accent-teal);
  background: var(--accent-teal-dim);
  border: 1px solid var(--accent-teal);
  border-radius: 999px;
  padding: 2px 9px;
}
.cogc-note {
  font-size: 0.74rem;
  color: var(--text-muted);
  text-align: center;
  line-height: 1.4;
  max-width: 540px;
}

/* Hero variant ----------------------------------------------------------- */
.cogc-versus--big {
  gap: 14px;
}
.cogc-versus--big .cogc-versus__row { gap: 40px; }
.cogc-versus--big .cogc-num { font-size: 5rem; }
.cogc-versus--big .cogc-eyebrow { font-size: 0.72rem; }
.cogc-versus--big .cogc-cap { font-size: 0.82rem; max-width: 200px; margin-top: 10px; }
.cogc-versus--big .cogc-sep { font-size: 2.2rem; margin-top: 42px; }
.cogc-versus--big .cogc-delta { font-size: 0.95rem; margin-top: 40px; padding: 4px 13px; }
.cogc-versus--big .cogc-note { font-size: 0.92rem; max-width: 620px; }
</style>
