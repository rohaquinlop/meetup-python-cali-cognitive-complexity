<script setup lang="ts">
defineProps<{
  image: string
  title: string
  description: string
  compact?: boolean
}>()
</script>

<template>
  <div
    class="giphy-card"
    :class="{ 'giphy-card--compact': compact }"
  >
    <img
      :src="image"
      class="giphy-card__image"
      alt=""
    />
    <div class="giphy-card__title">{{ title }}</div>
    <div class="giphy-card__description">{{ description }}</div>
  </div>
</template>

<style scoped>
.giphy-card {
  background: var(--bg-card);
  border: 1px solid var(--border-default);
  border-radius: 8px;
  padding: 18px 24px;
  text-align: center;
  max-width: 380px;
  overflow: hidden;
}

.giphy-card--compact {
  padding: 16px 14px;
}

.giphy-card__image {
  width: 100%;
  height: 140px;
  object-fit: contain;
  border-radius: 14px;
  margin-bottom: 12px;
}

.giphy-card--compact .giphy-card__image {
  margin-bottom: 10px;
}

.giphy-card__title {
  color: var(--text-primary);
  font-weight: 600;
  font-size: 1.1rem;
  margin-bottom: 6px;
}

.giphy-card--compact .giphy-card__title {
  font-size: 1rem;
}

.giphy-card__description {
  color: var(--text-muted);
  font-size: 0.78rem;
  line-height: 1.4;
}

.giphy-card--compact .giphy-card__description {
  font-size: 0.76rem;
}
</style>
