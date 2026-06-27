<script setup lang="ts">
// Weekly PyPI downloads of complexipy, 2024-02-11 .. 2026-06-14 (source: pepy.tech)
const data = [{d:"2024-02-11",v:3768},{d:"2024-02-18",v:512},{d:"2024-02-25",v:4601},{d:"2024-03-03",v:3069},{d:"2024-03-10",v:5232},{d:"2024-03-17",v:5814},{d:"2024-03-24",v:2554},{d:"2024-03-31",v:3790},{d:"2024-04-07",v:3911},{d:"2024-04-14",v:3918},{d:"2024-04-21",v:4522},{d:"2024-04-28",v:5601},{d:"2024-05-05",v:3548},{d:"2024-05-12",v:2630},{d:"2024-05-19",v:2424},{d:"2024-05-26",v:2591},{d:"2024-06-02",v:2967},{d:"2024-06-09",v:3621},{d:"2024-06-16",v:8373},{d:"2024-06-23",v:5437},{d:"2024-06-30",v:4244},{d:"2024-07-07",v:5449},{d:"2024-07-14",v:5289},{d:"2024-07-21",v:3011},{d:"2024-07-28",v:4700},{d:"2024-08-04",v:3441},{d:"2024-08-11",v:3767},{d:"2024-08-18",v:3013},{d:"2024-08-25",v:2977},{d:"2024-09-01",v:5197},{d:"2024-09-08",v:3576},{d:"2024-09-15",v:2583},{d:"2024-09-22",v:5262},{d:"2024-09-29",v:4192},{d:"2024-10-06",v:10783},{d:"2024-10-13",v:6745},{d:"2024-10-20",v:8214},{d:"2024-10-27",v:10852},{d:"2024-11-03",v:9652},{d:"2024-11-10",v:9504},{d:"2024-11-17",v:7808},{d:"2024-11-24",v:4270},{d:"2024-12-01",v:6066},{d:"2024-12-08",v:11971},{d:"2024-12-15",v:15595},{d:"2024-12-22",v:2801},{d:"2024-12-29",v:6010},{d:"2025-01-05",v:8618},{d:"2025-01-12",v:8348},{d:"2025-01-19",v:5263},{d:"2025-01-26",v:5869},{d:"2025-02-02",v:7320},{d:"2025-02-09",v:5745},{d:"2025-02-16",v:2652},{d:"2025-02-23",v:5073},{d:"2025-03-02",v:6992},{d:"2025-03-09",v:9324},{d:"2025-03-16",v:7481},{d:"2025-03-23",v:5522},{d:"2025-03-30",v:7624},{d:"2025-04-06",v:9594},{d:"2025-04-13",v:16048},{d:"2025-04-20",v:27876},{d:"2025-04-27",v:9276},{d:"2025-05-04",v:5735},{d:"2025-05-11",v:6437},{d:"2025-05-18",v:3795},{d:"2025-05-25",v:7100},{d:"2025-06-01",v:8259},{d:"2025-06-08",v:8139},{d:"2025-06-15",v:12931},{d:"2025-06-22",v:6620},{d:"2025-06-29",v:7615},{d:"2025-07-06",v:24778},{d:"2025-07-13",v:23548},{d:"2025-07-20",v:15437},{d:"2025-07-27",v:10920},{d:"2025-08-03",v:15244},{d:"2025-08-10",v:10183},{d:"2025-08-17",v:31128},{d:"2025-08-24",v:15160},{d:"2025-08-31",v:21264},{d:"2025-09-07",v:25673},{d:"2025-09-14",v:16130},{d:"2025-09-21",v:25217},{d:"2025-09-28",v:16656},{d:"2025-10-05",v:17670},{d:"2025-10-12",v:17498},{d:"2025-10-19",v:21041},{d:"2025-10-26",v:19279},{d:"2025-11-02",v:16695},{d:"2025-11-09",v:23783},{d:"2025-11-16",v:25340},{d:"2025-11-23",v:30459},{d:"2025-11-30",v:37406},{d:"2025-12-07",v:42051},{d:"2025-12-14",v:32705},{d:"2025-12-21",v:22246},{d:"2025-12-28",v:22200},{d:"2026-01-04",v:41698},{d:"2026-01-11",v:46862},{d:"2026-01-18",v:45983},{d:"2026-01-25",v:56785},{d:"2026-02-01",v:61853},{d:"2026-02-08",v:67517},{d:"2026-02-15",v:46029},{d:"2026-02-22",v:53183},{d:"2026-03-01",v:59136},{d:"2026-03-08",v:65837},{d:"2026-03-15",v:65355},{d:"2026-03-22",v:67420},{d:"2026-03-29",v:56529},{d:"2026-04-05",v:62334},{d:"2026-04-12",v:69547},{d:"2026-04-19",v:88533},{d:"2026-04-26",v:69985},{d:"2026-05-03",v:78685},{d:"2026-05-10",v:98984},{d:"2026-05-17",v:94434},{d:"2026-05-24",v:84650},{d:"2026-05-31",v:108125},{d:"2026-06-07",v:103944},{d:"2026-06-14",v:122487}]

const W = 780, H = 256, padL = 6, padR = 6, padT = 28, padB = 24
const n = data.length
const max = Math.max(...data.map(p => p.v))
const xx = (i: number) => padL + (i / (n - 1)) * (W - padL - padR)
const yy = (v: number) => padT + (1 - v / max) * (H - padT - padB)

const line = data.map((p, i) => `${i === 0 ? 'M' : 'L'}${xx(i).toFixed(1)} ${yy(p.v).toFixed(1)}`).join(' ')
const area = `${line} L${xx(n - 1).toFixed(1)} ${H - padB} L${xx(0).toFixed(1)} ${H - padB} Z`

// Annotation: the previous talk (Mar 2024) and today (Jun 2026)
const marchIdx = data.findIndex(p => p.d === '2024-03-17')
const nowIdx = n - 1
const mx = xx(marchIdx), my = yy(data[marchIdx].v)
const nx = xx(nowIdx), ny = yy(data[nowIdx].v)
</script>

<template>
  <div class="dl-chart">
    <svg :viewBox="`0 0 ${W} ${H}`" class="dl-svg" preserveAspectRatio="xMidYMid meet">
      <defs>
        <linearGradient id="dlFill" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="var(--accent-teal)" stop-opacity="0.32" />
          <stop offset="100%" stop-color="var(--accent-teal)" stop-opacity="0" />
        </linearGradient>
      </defs>

      <!-- baseline at the Mar-2024 level, to show how flat it was -->
      <line :x1="padL" :y1="my" :x2="W - padR" :y2="my"
            stroke="var(--border-default)" stroke-width="1" stroke-dasharray="3 5" />

      <path :d="area" fill="url(#dlFill)" />
      <path :d="line" fill="none" stroke="var(--accent-teal)" stroke-width="2"
            stroke-linejoin="round" stroke-linecap="round" />

      <!-- Mar 2024 marker -->
      <circle :cx="mx" :cy="my" r="4" fill="var(--bg-card)"
              stroke="var(--text-muted)" stroke-width="2" />
      <text :x="mx + 8" :y="my - 10" class="dl-tag dl-tag--muted">Marzo 2024</text>
      <text :x="mx + 8" :y="my + 4" class="dl-val dl-val--muted">5.8k / semana</text>

      <!-- Jun 2026 marker -->
      <circle :cx="nx" :cy="ny" r="9" fill="var(--accent-teal)" opacity="0.18" />
      <circle :cx="nx" :cy="ny" r="4.5" fill="var(--accent-teal)" />
      <text :x="nx - 8" :y="ny - 16" text-anchor="end" class="dl-tag">Junio 2026</text>
      <text :x="nx - 8" :y="ny - 2" text-anchor="end" class="dl-val">122k / semana</text>
    </svg>
  </div>
</template>

<style scoped>
.dl-chart { width: 100%; }
.dl-svg { width: 100%; height: auto; display: block; }
.dl-tag {
  font-family: 'Space Grotesk', sans-serif;
  font-size: 11px;
  font-weight: 600;
  fill: var(--accent-teal);
}
.dl-tag--muted { fill: var(--text-muted); }
.dl-val {
  font-family: 'JetBrains Mono', monospace;
  font-size: 11px;
  font-weight: 600;
  fill: var(--text-primary);
}
.dl-val--muted { fill: var(--text-very-muted); }
</style>
