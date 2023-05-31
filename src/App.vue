<template>
  <div>
    <HeaderVue></HeaderVue>
    <router-view></router-view>
    <FooterVue></FooterVue>
  </div>
  <!-- <component :is="currentPageComponent" :page-params="currentPageParams"></component> -->
  <!-- <MainPage v-if="currentPage === 'main'"></MainPage>
  <ProductPage v-else-if="currentPage === 'product'"></ProductPage>
  <NotFoundPage v-else ></NotFoundPage> -->
</template>

<script>
import HeaderVue from '@/components/HeaderVue.vue';
import FooterVue from '@/components/FooterVue.vue';
import { mapActions, mapMutations } from 'vuex';

export default {
  // name: 'App',

  components: { HeaderVue, FooterVue },
  created() {
    // this.$store.dispatch('loadCart');
    const userAccessKey = localStorage.getItem('userAccessKey');
    if (userAccessKey) {
      this.updateUserAccessKey(userAccessKey);
    }
    this.loadCart();
  },
  methods: {
    ...mapActions(['loadCart']),
    ...mapMutations(['updateUserAccessKey']),
  },
};
// import MainPage from './pages/MainPage.vue';
// import ProductPage from './pages/ProductPage.vue';
// import NotFoundPage from './pages/NotFoundPage.vue';
// import eventBus from './eventBus';

// const routes = {
//   main: 'MainPage',
//   product: 'ProductPage',
// };
// export default {
//   name: 'App',
//   // components: {ProductList}, //analogy
//   components: {
//     MainPage,
//     ProductPage,
//     NotFoundPage,
//   },
//   data() {
//     return {
//       currentPage: 'main',
//       currentPageParams: {},
//     };
//   },
//   methods: {
//     gotoPage(pageName, pageParams) {
//       this.currentPage = pageName;
//       this.currentPageParams = pageParams || {};
//     },
//   },
//   computed: {
//     currentPageComponent() {
//       return routes[this.currentPage] || 'NotFoundPage';
//     },
//   },
//   created() {
//     eventBus.$on('gotoPage', (pageName, pageParams) => this.gotoPage(pageName, pageParams));
//   },
// };
</script>
