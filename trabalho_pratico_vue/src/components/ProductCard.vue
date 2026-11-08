<script setup>
const props = defineProps({
  name: { type: String, required: true },
  price: { type: Number, required: true },
  image: { type: String, required: true },
  description: { type: String, default: '' },
  category: { type: String, default: 'Produto' },
  badge: { type: String, default: '' },
})

const emit = defineEmits(['add-to-cart'])

function formatPrice(value) {
  return new Intl.NumberFormat('pt-BR', {
    style: 'currency',
    currency: 'BRL',
  }).format(value)
}
</script>

<template>
  <article class="product-card">
    <div class="product-image-wrap">
      <span v-if="badge" class="product-badge">{{ badge }}</span>
      <img class="product-image" :src="image" :alt="`Imagem ilustrativa de ${name}`" loading="lazy" />
    </div>

    <div class="product-info">
      <div class="product-meta">
        <span class="product-category">{{ category }}</span>
      </div>
      <h3 class="product-name">{{ name }}</h3>
      <p class="product-description">{{ description }}</p>

      <div class="product-card-footer">
        <strong class="product-price">{{ formatPrice(price) }}</strong>
        <button
          class="add-button"
          type="button"
          :aria-label="`Adicionar ${name} ao carrinho`"
          @click="emit('add-to-cart')"
        >
          <span>Adicionar</span>
          <span class="button-plus" aria-hidden="true">+</span>
        </button>
      </div>
    </div>
  </article>
</template>
