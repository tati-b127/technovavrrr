<template>
      <div class="product__counter form__counter">
      <button type="button" aria-label="Убрать один товар" @click.prevent="dec(amount)">
        <svg width="10" height="10" fill="currentColor">
          <use xlink:href="#icon-minus"></use>
        </svg>
      </button>

      <label for="count">
        <input type="text" name="count" :value='amount'/>
      </label>

      <button type="button" aria-label="Добавить один товар" @click.prevent="inc(amount)">
        <svg width="10" height="10" fill="currentColor">
          <use xlink:href="#icon-plus"></use>
        </svg>
      </button>
    </div>

</template>
<script>
export default {
  props: ['productId', 'amount'],
  methods: {
    inc(amount) {
      this.$emit('update:amount', amount + 1);
    },
    dec(amount) {
      this.$emit('update:amount', amount - 1);
    },
  },
};

</script>
<style>
.form__counter button{
  cursor: pointer;
}
</style>
