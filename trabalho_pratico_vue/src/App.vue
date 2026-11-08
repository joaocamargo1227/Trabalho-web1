<script setup>
import { computed, ref } from 'vue'
import ProductCard from './components/ProductCard.vue'
import CartSummary from './components/CartSummary.vue'

const searchQuery = ref('')
const cart = ref([])
const notice = ref('')

const products = [
  {
    id: 1,
    name: 'Fone sem fio Pulse',
    price: 189.90,
    image: '/images/headphones.svg',
    category: 'Áudio',
    badge: 'Mais vendido',
    description: 'Som envolvente e conforto para acompanhar sua rotina.',
  },
  {
    id: 2,
    name: 'Mochila urbana Move',
    price: 139.90,
    image: '/images/backpack.svg',
    category: 'Acessórios',
    badge: 'Versátil',
    description: 'Espaço inteligente para trabalho, estudo e passeios.',
  },
  {
    id: 3,
    name: 'Smartwatch Flow',
    price: 229.90,
    image: '/images/watch.svg',
    category: 'Tecnologia',
    badge: 'Destaque',
    description: 'Acompanhe atividades e notificações em um só lugar.',
  },
  {
    id: 4,
    name: 'Garrafa térmica Urban',
    price: 69.90,
    image: '/images/bottle.svg',
    category: 'Dia a dia',
    badge: '',
    description: 'Design compacto para manter sua bebida por perto.',
  },
  {
    id: 5,
    name: 'Mouse sem fio Click',
    price: 89.90,
    image: '/images/mouse.svg',
    category: 'Tecnologia',
    badge: '',
    description: 'Precisão e praticidade para sua mesa de trabalho.',
  },
  {
    id: 6,
    name: 'Tênis casual Track',
    price: 249.90,
    image: '/images/sneaker.svg',
    category: 'Calçados',
    badge: 'Novo',
    description: 'Visual leve e confortável para usar todos os dias.',
  },
]

const filteredProducts = computed(() => {
  const term = searchQuery.value.trim().toLocaleLowerCase('pt-BR')
  if (!term) return products

  return products.filter((product) =>
    product.name.toLocaleLowerCase('pt-BR').includes(term),
  )
})

const cartItemCount = computed(() =>
  cart.value.reduce((total, item) => total + item.quantity, 0),
)

const cartSubtotal = computed(() =>
  cart.value.reduce((total, item) => total + item.price * item.quantity, 0),
)

function addToCart(product) {
  const existingItem = cart.value.find((item) => item.id === product.id)

  if (existingItem) {
    cart.value = cart.value.map((item) =>
      item.id === product.id ? { ...item, quantity: item.quantity + 1 } : item,
    )
  } else {
    cart.value = [...cart.value, { ...product, quantity: 1 }]
  }

  notice.value = `${product.name} adicionado ao carrinho.`
  window.setTimeout(() => {
    if (notice.value === `${product.name} adicionado ao carrinho.`) notice.value = ''
  }, 2200)
}

function changeCartQuantity({ productId, delta }) {
  cart.value = cart.value
    .map((item) => item.id === productId
      ? { ...item, quantity: item.quantity + delta }
      : item,
    )
    .filter((item) => item.quantity > 0)
}

function finishOrder() {
  notice.value = 'Pedido demonstrativo concluído! Nenhuma cobrança foi realizada.'
  cart.value = []
  window.setTimeout(() => { notice.value = '' }, 3500)
}
</script>

<template>
  <div class="app-shell">
    <header class="site-header">
      <a class="brand" href="#inicio" aria-label="Nuvem Store - página inicial">
        <span class="brand-mark">n.</span>
        <span>nuvem<span class="brand-light">store</span></span>
      </a>

      <nav class="main-nav" aria-label="Navegação principal">
        <a href="#catalogo">Catálogo</a>
        <a href="#sobre">Sobre o projeto</a>
      </nav>

      <a class="header-cart" href="#carrinho" aria-label="Ir para o carrinho">
        <svg viewBox="0 0 24 24" fill="none" aria-hidden="true">
          <path d="M5 8.5h14l1 12H4l1-12Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round" />
          <path d="M9 9V6a3 3 0 0 1 6 0v3" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" />
        </svg>
        <span>Meu carrinho</span>
        <strong>{{ cartItemCount }}</strong>
      </a>
    </header>

    <main id="inicio">
      <section class="hero-section">
        <div class="hero-copy">
          <span class="eyebrow hero-eyebrow"><span class="eyebrow-dot"></span> CATÁLOGO DEMONSTRATIVO</span>
          <h1>Boas escolhas para<br /><em>todos os seus dias.</em></h1>
          <p>Produtos práticos, funcionais e com personalidade. Explore a vitrine e monte seu pedido.</p>
          <a class="hero-link" href="#catalogo">Explorar produtos <span aria-hidden="true">↓</span></a>
        </div>
        <div class="hero-art" aria-hidden="true">
          <div class="hero-orbit orbit-one"></div>
          <div class="hero-orbit orbit-two"></div>
          <div class="hero-card hero-card-back"><span>DESIGN</span><strong>+</strong></div>
          <div class="hero-card hero-card-front"><span>EVERYDAY<br />ESSENTIALS</span><div class="hero-spark">✳</div></div>
          <div class="hero-floating-dot"></div>
        </div>
      </section>

      <section id="catalogo" class="catalog-layout">
        <div class="catalog-main">
          <div class="section-heading">
            <div>
              <span class="eyebrow">NOSSA SELEÇÃO</span>
              <h2>Encontre seu próximo <em>favorito.</em></h2>
            </div>
            <span class="result-count">{{ filteredProducts.length }} produtos</span>
          </div>

          <label class="search-box" for="product-search">
            <svg viewBox="0 0 24 24" fill="none" aria-hidden="true">
              <circle cx="10.8" cy="10.8" r="6.8" stroke="currentColor" stroke-width="1.7" />
              <path d="m16 16 4.5 4.5" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" />
            </svg>
            <input
              id="product-search"
              v-model="searchQuery"
              type="search"
              placeholder="Buscar produto pelo nome..."
              autocomplete="off"
            />
            <kbd>⌕</kbd>
          </label>

          <div v-if="filteredProducts.length" class="product-grid">
            <ProductCard
              v-for="product in filteredProducts"
              :key="product.id"
              :name="product.name"
              :price="product.price"
              :image="product.image"
              :description="product.description"
              :category="product.category"
              :badge="product.badge"
              @add-to-cart="addToCart(product)"
            />
          </div>
          <div v-else class="no-results">
            <span aria-hidden="true">⌕</span>
            <strong>Nenhum produto encontrado</strong>
            <p>Tente pesquisar por outro nome ou limpe o campo de busca.</p>
            <button type="button" @click="searchQuery = ''">Limpar busca</button>
          </div>
        </div>

        <aside id="carrinho" class="cart-aside">
          <CartSummary
            :items="cart"
            :item-count="cartItemCount"
            :subtotal="cartSubtotal"
            @change-quantity="changeCartQuantity"
            @checkout="finishOrder"
          />
        </aside>
      </section>

      <Transition name="toast">
        <div v-if="notice" class="toast-message" role="status" aria-live="polite">
          <span class="toast-check" aria-hidden="true">✓</span>
          {{ notice }}
        </div>
      </Transition>
    </main>

    <footer id="sobre" class="site-footer">
      <div class="footer-brand"><span class="brand-mark">n.</span><strong>nuvem<span class="brand-light">store</span></strong></div>
      <p>Projeto acadêmico desenvolvido com Vue.js 3, Vite, componentes e reatividade.</p>
      <span class="footer-caption">ENGENHARIA DE SOFTWARE · PROGRAMAÇÃO WEB I · 2026</span>
    </footer>
  </div>
</template>
