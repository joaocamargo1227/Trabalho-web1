<script setup>
defineProps({
  items: { type: Array, required: true },
  itemCount: { type: Number, required: true },
  subtotal: { type: Number, required: true },
})

const emit = defineEmits(['change-quantity', 'checkout'])

function formatPrice(value) {
  return new Intl.NumberFormat('pt-BR', {
    style: 'currency',
    currency: 'BRL',
  }).format(value)
}
</script>

<template>
  <section class="cart-panel" aria-labelledby="cart-title">
    <div class="cart-heading">
      <div>
        <span class="eyebrow">SEU PEDIDO</span>
        <h2 id="cart-title">Carrinho <span class="cart-count">{{ itemCount }}</span></h2>
      </div>
      <span class="cart-bag" aria-hidden="true">
        <svg viewBox="0 0 24 24" fill="none">
          <path d="M5 8.5h14l1 12H4l1-12Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round" />
          <path d="M9 9V6a3 3 0 0 1 6 0v3" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" />
        </svg>
      </span>
    </div>

    <div v-if="items.length === 0" class="cart-empty">
      <div class="empty-bag" aria-hidden="true">＋</div>
      <strong>Seu carrinho está vazio</strong>
      <p>Escolha seus produtos favoritos para começar.</p>
    </div>

    <div v-else class="cart-items">
      <article v-for="item in items" :key="item.id" class="cart-item">
        <img :src="item.image" :alt="`Imagem de ${item.name}`" class="cart-item-image" />
        <div class="cart-item-info">
          <strong>{{ item.name }}</strong>
          <span>{{ formatPrice(item.price) }}</span>
          <div class="quantity-control" :aria-label="`Quantidade de ${item.name}`">
            <button type="button" :aria-label="`Diminuir quantidade de ${item.name}`" @click="emit('change-quantity', { productId: item.id, delta: -1 })">−</button>
            <span>{{ item.quantity }}</span>
            <button type="button" :aria-label="`Aumentar quantidade de ${item.name}`" @click="emit('change-quantity', { productId: item.id, delta: 1 })">+</button>
          </div>
        </div>
        <strong class="cart-item-total">{{ formatPrice(item.price * item.quantity) }}</strong>
      </article>
    </div>

    <div class="cart-bottom">
      <div class="subtotal-row">
        <span>Subtotal</span>
        <strong>{{ formatPrice(subtotal) }}</strong>
      </div>
      <p class="shipping-note">Frete e condições de entrega não estão incluídos nesta demonstração.</p>
      <button class="checkout-button" type="button" :disabled="itemCount === 0" @click="emit('checkout')">
        Finalizar pedido <span aria-hidden="true">→</span>
      </button>
      <p class="demo-note">Demonstração acadêmica - nenhum pagamento será realizado.</p>
    </div>
  </section>
</template>
