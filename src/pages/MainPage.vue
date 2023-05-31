<template>
  <main class="content container">
    <div class="content__top content__top--catalog">
      <h1 class="content__title">Каталог</h1>
      <span class="content__info"> {{ countProducts }} товаров </span>
    </div>

    <div class="content__catalog">
      <ProductFilter
        :price-from.sync="filterPriceFrom"
        :price-to.sync="filterPriceTo"
        :category-id.sync="filterCategoryId"
        :color-id.sync="filterColorId"
      ></ProductFilter>
      <section class="catalog">
        <BaseLoader v-if="productsLoading" :loading="productsLoading"></BaseLoader>
        <div class="catalog__message" v-else-if="productsLoadingFailed">
          Произошла ошибка загрузки товаров =( <br />
          Попробуйте перезагрузить страницу <br />
          <button class="button button--primery button--back" @click.prevent="loadProducts">
            Перезагрузить
          </button>
        </div>
        <ProductList v-else :products="products"></ProductList>
        <!-- <ProductList :products="products"/> -->
        <!--//analogy-->
        <AppPagination
          v-model="page"
          :count="countProducts"
          :per-page="productsPerPage"
        ></AppPagination>
      </section>
    </div>
  </main>
</template>

<script>
// import products from '@/data/products';
import ProductList from '@/components/ProductList.vue';
import AppPagination from '@/components/AppPagination.vue';
import ProductFilter from '@/components/ProductFilter.vue';
import axios from 'axios';
import { API_BASE_URL } from '@/config';
import BaseLoader from '@/components/BaseLoader.vue';

export default {
  components: {
    ProductList,
    AppPagination,
    ProductFilter,
    BaseLoader,
  },
  data() {
    return {
      filterPriceFrom: 0,
      filterPriceTo: 0,
      filterCategoryId: 0,
      filterColorId: 0,
      page: 1,
      productsPerPage: 6,
      productsData: null,
      productsLoading: false,
      productsLoadingFailed: false,
      // products,
      // products: products, //analogy
    };
  },
  methods: {
    loadProducts() {
      this.productsLoading = true;
      this.productsLoadingFailed = false;
      clearTimeout(this.loadProductsTimer);
      this.loadProductsTimer = setTimeout(() => {
        axios
          .get(`${API_BASE_URL}/api/products`, {
            params: {
              page: this.page,
              limit: this.productsPerPage,
              categoryId: this.filterCategoryId,
              colorId: this.filterColorId,
              minPrice: this.filterPriceFrom,
              maxPrice: this.filterPriceTo,
            },
          })
          .then((response) => {
            this.productsData = response.data;
            this.productsLoading = false;
          })
          .catch(() => { this.productsLoadingFailed = true; })
          .then(() => {
            this.productsLoading = false;
          });
      }, 0);
    },
  },
  watch: {
    page() {
      this.loadProducts();
    },
    filterCategoryId() {
      this.loadProducts();
    },
    filterColorId() {
      this.loadProducts();
    },
    filterPriceFrom() {
      this.loadProducts();
    },
    filterPriceTo() {
      this.loadProducts();
    },
  },
  computed: {
    products() {
      return this.productsData
        ? this.productsData.items.map((product) => ({
          ...product,
          image: product.image.file.url,
        }))
        : [];
      // const offset = (this.page - 1) * this.productsPerPage;
      // return this.filterProducts.slice(offset, offset + this.productsPerPage);
    },
    countProducts() {
      return this.productsData ? this.productsData.pagination.total : 0;
      // return 21;
    },
    // filterProducts() {
    //   let filteredProducts = this.products;
    //   if (this.filterPriceFrom > 0) {
    //     filteredProducts = filteredProducts.filter(
    //       (product) => product.price > this.filterPriceFrom,
    //     );
    //   }
    //   if (this.filterPriceTo > 0) {
    //     filteredProducts = filteredProducts.filter((product) =>
    // product.price < this.filterPriceTo);
    //   }
    //   if (this.filterCategoryId) {
    //     filteredProducts = filteredProducts.filter(
    //       (product) => product.categoryId === this.filterCategoryId,
    //     );
    //   }
    //   if (this.filterColorId) {
    //     filteredProducts = filteredProducts.filter(
    //       (product) => product.colorId.includes(this.filterColorId),
    //     );
    //   }
    //   return filteredProducts;
    // },
  },
  created() {
    this.loadProducts();
  },
};
</script>
