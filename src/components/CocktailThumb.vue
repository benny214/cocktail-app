<template>
  <div class="cocktail__item">
    <RouterLink :to="`/cocktail/${cocktail.idDrink}`">
      <div
        class="cocktail__img"
        :style="`background-image: url(${cocktail.strDrinkThumb})`"
      ></div>
    </RouterLink>
    <div class="cocktail__name">{{ cocktail.strDrink }}</div>
  </div>
</template>
<script setup>
const props = defineProps({
  cocktail: Object,
  required: true,
});
</script>
<style lang="scss" scoped>
@import "../assets/styles/main";
.cocktail__item {
  width: calc((100% - 76px) / 3);
  margin-right: 38px;
  margin-bottom: 41px;
  &:nth-child(3n + 3) {
    margin-right: 0;
  }
}

.cocktail__img {
  width: 120px;
  height: 120px;
  background-repeat: no-repeat;
  background-position: 50% 50%;
  background-size: cover;
  margin: 0 auto;
}

.cocktail__name {
  width: 100%;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  margin-top: 15px;
  letter-spacing: 0.1em;
}
</style>
