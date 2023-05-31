<template>
  <main class="content container container--error">
    <h1
      class="error-title"
    >
      Страница не найдена
    </h1>
    <h2 class="error-title">:(</h2>
    <router-link :to="{name: 'main'}" class="button button--primery button--back"
      >Вернуться на главную</router-link
    >
  </main>
</template>
<script>
import gotoPage from '@/helpers/gotoPage';

export default {
  methods: {
    gotoPage,
  },
};
</script>
<style>

.error-title {
  text-align: center;
  font-family: 'PressStart';
}
.button--back:not(:disabled):hover {
  background-color: #222;
}
.button--back:not(:disabled):focus {
  background-color: #222;
}
.container--error {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
