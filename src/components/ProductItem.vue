<template>
  <li class="catalog__item">
    <router-link
      class="catalog__pic"
      :to="{name: 'product', params: {id: product.id}}"
    >
      <img :src="product.image" :srcset="product.image" :alt="product.title" />
    </router-link>

    <h3 class="catalog__title">
      <a href="#">
        {{ product.title }}
      </a>
    </h3>

    <span class="catalog__price"> {{ product.price | numberFormat }} ₽ </span>

    <ul class="colors colors--black">
      <li class="colors__item" v-for="(colorItem, index) in productColors" :key="colorItem.id">
        <label :for="colorItem.id" class="colors__label">
          <input
            class="colors__radio sr-only"
            type="radio"
            :value="colorItem.id"
            :v-model="index"
          />
          <span class="colors__value" :style="{ 'background-color': colorItem.code }"> </span>
        </label>
      </li>
    </ul>
  </li>
</template>
<script>
// import eventBus from '@/eventBus';
import gotoPage from '@/helpers/gotoPage';
// import colors from '@/data/colors';
import numberFormat from '@/helpers/numberFormat';

export default {
  computed: {
    productColors() {
      return this.product.colors;
    },
  },

  data() {
    return {
      // color: this.product.colors[0].id, // что возвращать в v-model
      currentColor: this.product,
    };
  },
  filters: {
    numberFormat,
  },
  methods: { // переносим в отдельный файл /helpers/gotoPage.js
    // gotoPage(pageName, pageParams) {
    //   eventBus.$emit('gotoPage', pageName, pageParams);
    // },
    gotoPage,
  },
  props: ['product'],
};
</script>
