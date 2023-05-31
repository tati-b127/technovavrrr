<template>
  <li class="cart__item product">
    <div class="product__pic">
      <img
        :src="item.product.image"
        width="120"
        height="120"
        :srcset="item.product.image"
        :alt="item.product.title"
      />
    </div>
    <h3 class="product__title">{{ item.product.title }}</h3>
    <!--  <p class="product__info">Объем: <span>128GB</span></p> -->
    <span class="product__code"> Артикул: {{ item.product.id }} </span>
    <ChangeAmount :item="item" :amount.sync="amount"></ChangeAmount>
    <!-- <div class="product__counter form__counter">
      <button type="button" aria-label="Убрать один товар">
        <svg width="10" height="10" fill="currentColor">
          <use xlink:href="#icon-minus"></use>
        </svg>
      </button>

      <label for="count">
        <input type="text" v-model="amount" name="count" />
      </label>

      <button type="button" aria-label="Добавить один товар">
        <svg width="10" height="10" fill="currentColor">
          <use xlink:href="#icon-plus"></use>
        </svg>
      </button>
    </div> -->

    <b class="product__price"> {{ (item.amount * item.product.price) | numberFormat }} ₽ </b>

    <button
      class="product__del button-del"
      :disabled="cartItemDeleting"
      type="button" style="cursor: pointer;"
      aria-label="Удалить товар из корзины"
      @click.prevent="deleteProduct"
    >
      <svg width="20" height="20" fill="currentColor">
        <use xlink:href="#icon-close"></use>
      </svg>
      <span v-if="cartItemDeletedFailed">Ошибка</span>
    </button>
  </li>
</template>
<script>
import numberFormat from '@/helpers/numberFormat';
import { mapActions, mapMutations } from 'vuex';
import ChangeAmount from '@/components/ChangeAmount.vue';

export default {
  data() {
    return {
      cartItemDeleting: false,
      cartItemDeletedFailed: false,
      // disabled: true,
    };
  },

  filters: { numberFormat },
  props: ['item'],
  components: { ChangeAmount },
  computed: {
    amount: {
      get() {
        return this.item.amount;
      },
      set(value) {
        this.$store.dispatch('updateCartProductAmount', {
          productId: this.item.productId,
          amount: value,
        });
      },
    },
  },
  methods: {
    ...mapMutations({ deleteProductFromCart: 'deleteCartProduct' }),
    ...mapActions(['deleteCartProduct']),
    deleteProduct() {
      this.cartItemDeleting = true;
      this.deleteCartProduct({ productId: this.item.productId })
        .catch(() => { this.cartItemDeletedFailed = true; })
        .then(() => {
          this.cartItemDeleting = false;
          this.deleteProductFromCart(this.item.productId);
        });
    },
    // deleteProduct(productId) {
    //   this.$store.commit('deleteCartProduct', productId);
    // },
  },
};
</script>
