<template>
  <main class="content container">
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link" :to="{ name: 'main' }"> Каталог </router-link>
        </li>
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link" :to="{ name: 'cart' }"> Корзина </router-link>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link"> Оформление заказа </a>
        </li>
      </ul>

      <h1 class="content__title">Корзина</h1>
      <span class="content__info"> {{ $store.state.cartProducts.length }} товара </span>
    </div>

    <section class="cart">
      <!-- <BaseLoader v-if="orderPageLoading" :loading="orderPageLoading"></BaseLoader> -->
      <form class="cart__form form" action="#" method="POST" @submit.prevent="order">
        <div class="cart__field">
          <div class="cart__data">
            <BaseFormText
              title="ФИО"
              :error="formError.name"
              placeholder="Введите ваше полное имя"
              name="name"
              type="text"
              v-model="formData.name"
            ></BaseFormText>
            <BaseFormText
              title="Адрес доставки"
              :error="formError.address"
              placeholder="Введите ваш адрес"
              type="text"
              name="address"
              v-model="formData.address"
            >
            </BaseFormText>
            <BaseFormText
              title="Телефон"
              :error="formError.phone"
              type="tel"
              name="phone"
              placeholder="Введите ваш телефон"
              v-model="formData.phone"
            >
            </BaseFormText>
            <BaseFormText
              title="Email"
              :error="formError.email"
              type="email"
              name="email"
              placeholder="Введите ваш Email"
              v-model="formData.email"
            >
            </BaseFormText>
            <BaseFormTextarea
              title="Комментарий к заказу"
              :error="formError.comment"
              name="comment"
              placeholder="Ваши пожелания"
              v-model="formData.comment"
            >
            </BaseFormTextarea>
          </div>

          <div class="cart__options">
            <h3 class="cart__title">Доставка</h3>
            <ul class="cart__options options">
              <li class="options__item">
                <label for="delivery" class="options__label">
                  <input
                    class="options__radio sr-only"
                    type="radio"
                    name="delivery"
                    value="0"
                    checked=""
                  />
                  <span class="options__value"> Самовывоз <b>бесплатно</b> </span>
                </label>
              </li>
              <li class="options__item">
                <label for="delivery" class="options__label">
                  <input class="options__radio sr-only" type="radio" name="delivery" value="500" />
                  <span class="options__value"> Курьером <b>500 ₽</b> </span>
                </label>
              </li>
            </ul>

            <h3 class="cart__title">Оплата</h3>
            <ul class="cart__options options">
              <li class="options__item">
                <label for="pay" class="options__label">
                  <input
                    class="options__radio sr-only"
                    type="radio"
                    name="pay"
                    value="card"
                    checked=""
                  />
                  <span class="options__value"> Картой при получении </span>
                </label>
              </li>
              <li class="options__item">
                <label for="pay" class="options__label">
                  <input class="options__radio sr-only" type="radio" name="pay" value="cash" />
                  <span class="options__value"> Наличными при получении </span>
                </label>
              </li>
            </ul>
          </div>
        </div>
        <OrderItem
          :products="products"
          :totalPrice="totalPrice"
          :amount="amount"
          :orderLoading="orderLoading"
          :btnSend="true"
        ></OrderItem>
        <div class="cart__error form__error-block" v-if="formErrorMessage">
          <h4>Заявка не отправлена!</h4>
          <p>
            {{ formErrorMessage }}
          </p>
        </div>
      </form>
    </section>
  </main>
</template>
<script>
import numberFormat from '@/helpers/numberFormat';
import { mapGetters } from 'vuex';
import BaseFormText from '@/components/BaseFormText.vue';
import BaseFormTextarea from '@/components/BaseFormTextarea.vue';
import axios from 'axios';
import { API_BASE_URL } from '@/config';
import OrderItem from '@/components/OrderItem.vue';
// import BaseLoader from '@/components/BaseLoader.vue';

export default {
  data() {
    return {
      formData: {},
      formError: {
        // name: 'Значение не должно быть пустым',
        // address: 'Значение не должно быть пустым',
        // phone: 'Телефон должен быть не менее 11 символов',
        // email: 'Неверный формат Email',
        // comment: 'Неуказан комментарий',
      },
      formErrorMessage: '',
      orderLoading: false,
      // orderPageLoading: true,
      // amount: this.products.amount,
    };
  },
  components: {
    BaseFormText, BaseFormTextarea, OrderItem,
  },
  filters: {
    numberFormat,
  },
  computed: {
    ...mapGetters({ products: 'cartDetailProducts', totalPrice: 'cartTotalPrice' }),
    amount() {
      return this.products.map((item) => item.amount).reduce((acc, i) => acc + i, 0);
    },
  },
  // created() {
  //   this.orderPageLoading = false;
  // },
  methods: {
    order() {
      this.formError = {};
      this.formErrorMessage = '';
      this.orderLoading = true;
      axios
        .post(
          `${API_BASE_URL}/api/orders`,
          {
            ...this.formData,
          },
          {
            params: {
              userAccessKey: this.$store.state.userAccessKey,
            },
          },
        )
        .then((response) => {
          this.$store.commit('resetCart');
          this.$store.commit('updateOrderInfo', response.data);
          this.$router.push({ name: 'orderInfo', params: { id: response.data.id } });
        })
        .catch((error) => {
          this.formError = error.response.data.error.request || {};
          this.formErrorMessage = error.response.data.error.message;
        })
        .then(() => {
          this.orderLoading = false;
        });
    },
  },
};
</script>
