<template>
  <main class="content container container--error" v-if="productLoading">
    <BaseLoader :loading="productLoading"></BaseLoader>
  </main>
  <main class="content container container--error" v-else-if="!productData">
    <h1 class="error-title">Не удалось загрузить товар =(</h1>
  </main>

  <main class="content container" v-else>
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link" :to="{ name: 'main' }"> Каталог </router-link>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link" href="#" @click.prevent="$router.push({ name: 'main' })">
            {{ category.title }}
          </a>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link"> {{ product.title }} </a>
        </li>
      </ul>
    </div>

    <section class="item">
      <div class="item__pics pics">
        <div class="pics__wrapper">
          <img
            width="570"
            height="570"
            :src="product.image.file.url"
            :srcset="product.image.file.url"
            :alt="product.title"
          />
        </div>
        <ul class="pics__list">
          <li class="pics__item">
            <a href="" class="pics__link pics__link--current">
              <img width="98" height="98" :src="product.image.file.url" :alt="product.title" />
            </a>
          </li>
          <li class="pics__item">
            <a href="" class="pics__link">
              <img width="98" height="98" :src="product.image.file.url" :alt="product.title" />
            </a>
          </li>
          <li class="pics__item">
            <a href="" class="pics__link">
              <img width="98" height="98" :src="product.image.file.url" :alt="product.title" />
            </a>
          </li>
          <li class="pics__item">
            <a class="pics__link" href="#">
              <img width="98" height="98" :src="product.image.file.url" :alt="product.title" />
            </a>
          </li>
        </ul>
      </div>

      <div class="item__info">
        <span class="item__code">Артикул: {{ product.id }}</span>
        <h2 class="item__title">{{ product.title }}</h2>
        <div class="item__form">
          <form class="form" method="POST" @submit.prevent="addToCart">
            <b class="item__price"> {{ (product.price * productAmount) | numberFormat }} ₽ </b>

            <fieldset class="form__block">
              <legend class="form__legend">Цвет:</legend>
              <ul class="colors">
                <li class="colors__item" v-for="colorItem in colors" :key="colorItem.id">
                  <label :for="colorItem.id" class="colors__label">
                    <input
                      class="colors__radio sr-only"
                      type="radio"
                      :value="colorItem.id"
                      v-model="currentColorId"
                    />
                    <!-- <input
                      class="colors__radio sr-only"
                      type="radio"
                      :value="colorItem.id"
                      :checked="index"
                      v-model="color"
                    /> -->
                    <span class="colors__value" :style="{ 'background-color': colorItem.code }">
                    </span>
                  </label>
                </li>
              </ul>
            </fieldset>

            <fieldset class="form__block">
              <legend class="form__legend">Объемб в ГБ:</legend>

              <ul class="sizes sizes--primery">
                <li class="sizes__item">
                  <label for="size-item" class="sizes__label">
                    <input class="sizes__radio sr-only" type="radio" name="sizes-item" value="32" />
                    <span class="sizes__value"> 32gb </span>
                  </label>
                </li>
                <li class="sizes__item">
                  <label for="size-item" class="sizes__label">
                    <input class="sizes__radio sr-only" type="radio" name="sizes-item" value="64" />
                    <span class="sizes__value"> 64gb </span>
                  </label>
                </li>
                <li class="sizes__item">
                  <label for="size-item" class="sizes__label">
                    <input
                      class="sizes__radio sr-only"
                      type="radio"
                      name="sizes-item"
                      value="128"
                      checked=""
                    />
                    <span class="sizes__value"> 128gb </span>
                  </label>
                </li>
              </ul>
            </fieldset>

            <div class="item__row">
              <ChangeAmount :amount.sync="productAmount" :product-id="product.id"></ChangeAmount>
              <!-- <div class="form__counter">
                <button
                  type="button"
                  aria-label="Убрать один товар"
                  @click.prevent="dec"
                >
                  <svg width="12" height="12" fill="currentColor">
                    <use xlink:href="#icon-minus"></use>
                  </svg>
                </button>
                <label for="count">
                  <input type="text" value="1" name="count" v-model.number="productAmount" />
                </label>

                <button
                  type="button"
                  aria-label="Добавить один товар"
                  @click.prevent="inc"
                >
                  <svg width="12" height="12" fill="currentColor">
                    <use xlink:href="#icon-plus"></use>
                  </svg>
                </button>
              </div> -->

              <button class="button button--primery" type="submit" :disabled="productAddSending">
                В корзину
              </button>
              <div v-show="productAdded">Товар в корзине!</div>
              <div v-show="productAddSending">Добавляем товар в корзину...</div>
            </div>
          </form>
        </div>
      </div>

      <div class="item__desc">
        <ul class="tabs">
          <li class="tabs__item">
            <a class="tabs__link tabs__link--current"> Описание </a>
          </li>
          <li class="tabs__item">
            <a class="tabs__link" href="#"> Характеристики </a>
          </li>
          <li class="tabs__item">
            <a class="tabs__link" href="#"> Гарантия </a>
          </li>
          <li class="tabs__item">
            <a class="tabs__link" href="#"> Оплата и доставка </a>
          </li>
        </ul>

        <div class="item__content">
          <p>
            Навигация GPS, ГЛОНАСС, BEIDOU Galileo и QZSS<br />
            Синхронизация со смартфоном<br />
            Связь по Bluetooth Smart, ANT+ и Wi-Fi<br />
            Поддержка сторонних приложений<br />
          </p>

          <a href="#"> Все характеристики </a>

          <h3>Что это?</h3>

          <p>
            Wahoo ELEMNT BOLT GPS – это велокомпьютер, который позволяет оптимизировать свои
            велотренировки, сделав их максимально эффективными. Wahoo ELEMNT BOLT GPS
            синхронизируется с датчиками по ANT+, объединяя полученную с них информацию. Данные
            отображаются на дисплее, а также сохраняются на смартфоне. При этом на мобильное
            устройство можно установить как фирменное приложение, так и различные приложения
            сторонних разработчиков. Велокомпьютер точно отслеживает местоположение, принимая сигнал
            с целого комплекса спутников. Эта информация позволяет смотреть уже преодоленные
            маршруты и планировать новые велопрогулки.
          </p>

          <h3>Дизайн</h3>

          <p>
            Велокомпьютер Wahoo ELEMNT BOLT очень компактный. Размеры устройства составляют всего
            74,6 x 47,3 x 22,1 мм. что не превышает габариты смартфона. Корпус гаджета выполнен из
            черного пластика. На обращенной к пользователю стороне расположен дисплей диагональю 56
            мм. На дисплей выводятся координаты и скорость, а также полученная со смартфона и
            синхронизированных датчиков информация: интенсивность, скорость вращения педалей, пульс
            и т.д. (датчики не входят в комплект поставки). Корпус велокомпьютера имеет степень
            защиты от влаги IPX7. Это означает, что устройство не боится пыли, а также выдерживает
            кратковременное (до 30 минут) погружение в воду на глубину не более 1 метра.
          </p>
        </div>
      </div>
    </section>
  </main>
</template>
<script>
// import products from '@/data/products';
// import categories from '@/data/categories';
import gotoPage from '@/helpers/gotoPage';
import numberFormat from '@/helpers/numberFormat';
// import colors from '@/data/colors';
import ChangeAmount from '@/components/ChangeAmount.vue';
import axios from 'axios';
import { API_BASE_URL } from '@/config';
import { mapActions } from 'vuex';
import BaseLoader from '@/components/BaseLoader.vue';

export default {
  data() {
    return {
      productAmount: 1,
      currentColorId: null,
      productData: null,
      productLoading: false,
      productLoadingError: false,
      productAdded: false,
      productAddSending: false,
    };
  },
  // props: ['pageParams'],
  filters: {
    numberFormat,
  },
  watch: {
    colorId(value) {
      this.currentColorId = value;
    },
    // eslint-disable-next-line func-names
    // '$route.params.id': function () { this.loadProduct(); },
    '$route.params.id': {
      handler() {
        this.loadProduct();
      },
      immediate: true,
    },
  },
  components: { ChangeAmount, BaseLoader },
  computed: {
    product() {
      return this.productData;
    },
    category() {
      return this.productData.category;
    },
    colors() {
      return this.productData.colors;
    },

    // product() {
    //   // return products.find((product) => product.id === this.pageParams.id);
    //   return products.find((product) => product.id === this.$route.params.id);
    // },
    // // productAmount() {
    // //   return 1;
    // // },
    // category() {
    //   return categories.find((category) => category.id === this.product.categoryId);
    // },
    // colors() {
    //   return colors;
    // },
    // productColors() {
    //   return colors.filter((color) => this.product.colorId.includes(color.id));
    // },
  },
  methods: {
    ...mapActions(['addProductToCart']),
    gotoPage,
    addToCart() {
      this.productAdded = false;
      this.productAddSending = true;
      this.addProductToCart({
        productId: this.product.id,
        amount: this.productAmount,
      })
        .then(() => {
          this.productAdded = true;
          this.productAddSending = false;
        });
      // this.$store.commit('addProductToCart', {
      //   productId: this.product.id,
      //   amount: this.productAmount,
      // });
    },
    loadProduct() {
      this.productLoading = true;
      axios
        .get(`${API_BASE_URL}/api/products/${this.$route.params.id}`)
        .then((response) => {
          this.productData = response.data;
        })
        .catch(() => {
          this.productLoadingError = true;
        })
        .then(() => {
          this.productLoading = false;
        });
    },
  },
  created() {
    this.loadProduct();
  },
};
</script>
