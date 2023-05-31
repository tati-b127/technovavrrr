<template>
  <main class="content container">
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link" :to="{ name: 'main' }"> Каталог </router-link>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link"> Корзина </a>
        </li>
      </ul>

      <h1 class="content__title">Корзина</h1>
      <span class="content__info"> {{ $store.state.cartProducts.length }} товара </span>
    </div>

    <section class="cart">
      <BaseLoader v-if="cartProductsLoading" :loading="cartProductsLoading"></BaseLoader>
      <div class="catalog__message" v-else-if="cartProductsLoadingFailed">
        Произошла ошибка загрузки товаров =( <br />
        Попробуйте перезагрузить страницу <br />
        <button
          class="button button--primery button--back"
          @click.prevent="$router.go($router.currentRoute)"
        >
          Перезагрузить
        </button>
      </div>

      <form v-else class="cart__form form" action="#" method="POST">
        <div class="cart__field">
          <ul class="cart__list">
            <CartItem v-for="item in products" :key="item.productId" :item="item"></CartItem>
          </ul>
        </div>

        <div class="cart__block">
          <p class="cart__desc">Мы&nbsp;посчитаем стоимость доставки на&nbsp;следующем этапе</p>
          <p class="cart__price">
            Итого: <span>{{ totalPrice | numberFormat }} ₽</span>
          </p>

          <router-link
            tag="button"
            :to="{ name: 'order' }"
            class="cart__button button button--primery"
            type="submit"
            :disabled="$store.state.cartProducts.length <= 0"
            >Оформить заказ
          </router-link>
          <!-- <router-link
            tag="button"
            :to="{ name: 'order' }"
            class="cart__button button button--primery"
            type="submit"
            :disabled="emptyCart"
            >Оформить заказ
          </router-link> -->
        </div>
      </form>
    </section>
  </main>
</template>
<script>
import numberFormat from '@/helpers/numberFormat';
import { mapGetters, mapActions } from 'vuex';
import CartItem from '@/components/CartItem.vue';
import BaseLoader from '@/components/BaseLoader.vue';

export default {
  data() {
    return {
      cartProductsLoading: false,
      cartProductsLoadingFailed: false,
      // disabled: true,
    };
  },
  filters: { numberFormat },
  components: { CartItem, BaseLoader },
  computed: {
    ...mapGetters({ products: 'cartDetailProducts', totalPrice: 'cartTotalPrice' }),
    // emptyCart() {
    //   if (this.$store.state.cartProducts.length <= 0) return true;
    //   if (this.$store.state.cartProducts.length > 0) return false;
    // },
    // products() {
    //   return this.$store.getters.cartDetailProducts;
    // },
  },
  created() {
    this.cartProductsLoading = true;
    this.cartProductsLoadingFailed = false;

    this.loadCart()
      // eslint-disable-next-line no-return-assign
      .catch(() => (this.cartProductsLoadingFailed = true))
      // eslint-disable-next-line no-return-assign
      .then(() => (this.cartProductsLoading = false));
  },
  methods: {
    ...mapActions(['loadCart']),
  },
};
</script>
