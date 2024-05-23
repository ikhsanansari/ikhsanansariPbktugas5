<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title>
          Toko Parfum
        </q-toolbar-title>
        <q-btn flat round icon="home" @click="goHome"></q-btn>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

    <q-footer class="text-center q-pa-md">
      &copy; 2024 Toko Parfum
    </q-footer>
  </q-layout>
</template>

<script>
import { defineComponent, reactive } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import { createRouter, createWebHistory } from 'vue-router';
import { Quasar, QLayout, QHeader, QFooter, QPageContainer, QPage, QToolbar, QToolbarTitle, QBtn, QList, QItem, QItemSection, QItemLabel, QCarousel, QCarouselSlide, QCard, QCardSection, QCardActions, QImg } from 'quasar';

// Data Produk
const products = [
  {
    id: 1,
    name: 'Parfum A',
    description: 'Deskripsi Parfum A',
    price: 500000,
    image: 'https://via.placeholder.com/150'
  },
  {
    id: 2,
    name: 'Parfum B',
    description: 'Deskripsi Parfum B',
    price: 600000,
    image: 'https://via.placeholder.com/150'
  },
  {
    id: 3,
    name: 'Parfum C',
    description: 'Deskripsi Parfum C',
    price: 700000,
    image: 'https://via.placeholder.com/150'
  }
];

// Home Component
const Home = defineComponent({
  setup() {
    return { products };
  },
  methods: {
    viewProduct(id) {
      this.$router.push(`/product/${id}`);
    }
  },
  template: `
    <q-page class="q-pa-md">
      <q-carousel v-model="slide" arrows infinite swipeable>
        <q-carousel-slide v-for="(product, index) in products" :key="index" :name="index" :img-src="product.image" />
      </q-carousel>
      <div class="q-mt-xl">
        <q-list bordered padding>
          <q-item v-for="product in products" :key="product.id" clickable @click="viewProduct(product.id)">
            <q-card>
              <q-img :src="product.image" :alt="product.name" ratio="4/3" />
              <q-card-section>
                <div class="text-h6">{{ product.name }}</div>
                <div>{{ product.price | currency }}</div>
              </q-card-section>
              <q-card-actions align="right">
                <q-btn flat label="Detail" @click="viewProduct(product.id)" />
              </q-card-actions>
            </q-card>
          </q-item>
        </q-list>
      </div>
    </q-page>
  `,
  data() {
    return {
      slide: 0,
      products
    };
  }
});

// Product Detail Component
const ProductDetail = defineComponent({
  setup() {
    const route = useRoute();
    const productId = route.params.id;
    const product = products.find(p => p.id == productId);
    return { product };
  },
  methods: {
    goBack() {
      this.$router.push('/');
    }
  },
  template: `
    <q-page class="q-pa-md">
      <q-card v-if="product">
        <q-img :src="product.image" :alt="product.name" ratio="16/9"></q-img>
        <q-card-section>
          <div class="text-h6">{{ product.name }}</div>
          <div>{{ product.price | currency }}</div>
          <div>{{ product.description }}</div>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn flat label="Kembali" @click="goBack"></q-btn>
        </q-card-actions>
      </q-card>
    </q-page>
  `
});

// Router
const routes = [
  { path: '/', component: Home },
  { path: '/product/:id', component: ProductDetail }
];

const router = createRouter({
  history: createWebHistory(),
  routes
});

// Currency Filter
function currency(value) {
  return new Intl.NumberFormat('id-ID', {
    style: 'currency',
    currency: 'IDR'
  }).format(value);
}

export default defineComponent({
  name: 'App',
  setup() {
    const app = reactive({});
    return { app };
  },
  methods: {
    goHome() {
      this.$router.push('/');
    }
  },
  filters: {
    currency
  },
  router,
  components: {
    QLayout,
    QHeader,
    QFooter,
    QPageContainer,
    QPage,
    QToolbar,
    QToolbarTitle,
    QBtn,
    QList,
    QItem,
    QItemSection,
    QItemLabel,
    QCarousel,
    QCarouselSlide,
    QCard,
    QCardSection,
    QCardActions,
    QImg
  }
});
</script>

<style>
@import '~quasar/src/css/index.sass';
</style>
