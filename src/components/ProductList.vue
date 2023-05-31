<template>
  <ul class="catalog__list">
      <ProductItem :product="product" v-for="(product) in products" :key="product.id"/>
    <!-- <li class="catalog__item" v-for="(product, index) in products" :key="index">
      <a class="catalog__pic" href="#">
        <img :src="product.imageSrc" :srcset="product.imageSrcSet" :alt="product.title" />
      </a>

      <h3 class="catalog__title">
        <a href="#">
          {{ product.title }}
        </a>
      </h3>

      <span class="catalog__price">
        {{ product.price }}
      </span>

      <ul class="colors colors--black">
        <li class="colors__item">
          <label for="color-1" class="colors__label">
            <input
              class="colors__radio sr-only"
              type="radio"
              name="color-1"
              value="#73B6EA"
              checked=""
            />
            <span class="colors__value" style="background-color: #73b6ea"> </span>
          </label>
        </li>
        <li class="colors__item">
          <label for="color-1" class="colors__label">
            <input class="colors__radio sr-only" type="radio" name="color-1" value="#8BE000" />
            <span class="colors__value" style="background-color: #8be000"> </span>
          </label>
        </li>
        <li class="colors__item">
          <label for="color-1" class="colors__label">
            <input class="colors__radio sr-only" type="radio" name="color-1" value="#222" />
            <span class="colors__value" style="background-color: #222"> </span>
          </label>
        </li>
      </ul>
    </li> -->
  </ul>
</template>

<script>
import ProductItem from './ProductItem.vue';

export default {
  components: {
    ProductItem,
  },
  // props: ['products', 'product'],
  props: ['products'],
};
</script>
