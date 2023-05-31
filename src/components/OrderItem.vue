<template>
  <div class="cart__block">
    <ul class="cart__orders">
      <li class="cart__order" v-for="item in products" :key="item.productId">
        <h3>{{ item.product.title }}</h3>
        <b
          >{{
            item.product.price * item.amount || (item.quantity * item.product.price) | numberFormat
          }}
          ₽</b
        >
        <span>Артикул: {{ item.product.id }}</span>
        <span> {{ item.amount || item.quantity }} шт.</span>
      </li>
    </ul>

    <div class="cart__total">
      <p>Доставка: <b>500 ₽</b></p>
      <p>
        Итого: <b>{{ amount }}</b> товара на сумму <b>{{ totalPrice | numberFormat }} ₽</b>
      </p>
    </div>

    <button
      :disabled="orderLoading"
      class="cart__button button button--primery"
      type="submit"
      v-if="btnSend"
    >
      Оформить заказ
    </button>
    <div class="cart__loader" v-if="orderLoading">
      <div class="loader">
        <div class="wBall" id="wBall_1">
          <div class="wInnerBall"></div>
        </div>
        <div class="wBall" id="wBall_2">
          <div class="wInnerBall"></div>
        </div>
        <div class="wBall" id="wBall_3">
          <div class="wInnerBall"></div>
        </div>
        <div class="wBall" id="wBall_4">
          <div class="wInnerBall"></div>
        </div>
        <div class="wBall" id="wBall_5">
          <div class="wInnerBall"></div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
// import { mapGetters } from 'vuex';
import numberFormat from '@/helpers/numberFormat';

export default {
  // data() {
  //   return {
  //     orderLoading: false,
  //   };
  // },
  props: ['products', 'totalPrice', 'amount', 'orderLoading', 'btnSend'],
  // computed: {
  //   ...mapGetters({ products: 'cartDetailProducts', totalPrice: 'cartTotalPrice' }),
  //   amount() {
  //     return this.products.map((item) => item.amount).reduce((acc, i) => acc + i, 0);
  //   },
  // },
  filters: { numberFormat },
};
</script>
<style lang="scss">
.cart__loader {
  position: absolute;
  margin-top: -45px;
  margin-left: 20px;
  z-index: 100;
}
.cart__loader .loader {
  width: 20px;
  height: 20px;
  margin-top: 0;
}
.cart__loader .wBall {
  width: 20px;
  height: 20px;
}
.cart__loader .loader .wInnerBall {
  width: 2px;
  height: 2px;
}
</style>
