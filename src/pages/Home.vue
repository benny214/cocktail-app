<template>
  <AppLayout imgUrl="'/src/assets/img/bg-1.jpg'" :backFunc="removeIngredient" :isBackBtnVisible="!!ingredient">
    <div class="info">
      <div v-if="!ingredient || !cocktails" class="info__inner">
        <h1 class="info__title">Choose your drink</h1>
        <div class="info__select">
          <el-select
            v-model="rootStore.ingredient"
            placeholder="Choose main ingredient"
            class="select"
            @change="getCocktails"
            filterable
            allow-create
          >
            <el-option
              v-for="item in ingredients"
              :key="item.strIngredient1"
              :label="item.strIngredient1"
              :value="item.strIngredient1"
            />
          </el-select>
        </div>
        <div class="info__text">
          Try our delicious cocktail recipes for every occasion. Find delicious
          cocktail recipes by ingredient through our cocktail generator.
        </div>

        <div class="info__img">
          <img src="/src/assets/img/coctails.png" class="img" />
        </div>
      </div>

      <div v-else class="info__inner">
        <h1 class="info__title">COCKTAILS WITH {{ ingredient }}</h1>
        <div class="coctails__list">
          <CocktailThumb
            v-for="cocktail in cocktails"
            :key="cocktail.idDrink"
            :cocktail="cocktail"
          />
        </div>
      </div>
    </div>
  </AppLayout>
</template>
<script setup>
import AppLayout from "../components/AppLayout.vue";
import { useRootStore } from "../stores/root";
import { storeToRefs } from "pinia";
import CocktailThumb from "../components/CocktailThumb.vue";

const rootStore = useRootStore();
rootStore.getIngredients();

const { ingredients, ingredient, cocktails } = storeToRefs(rootStore);

function getCocktails() {
  rootStore.getCocktails(rootStore.ingredient);
}

function removeIngredient() {
  rootStore.setIngredient(null)
}
</script>
<style lang="scss" scoped>
@import "@/assets/styles/main.scss";

.info {
  display: flex;
  align-items: center;
  justify-content: center;
  &__inner {
    padding: 80px 0;
    text-align: center;
  }
  &__select {
    margin: 50px auto;
    width: 220px;
  }
  &__text {
    color: $text-muted;
    margin: 0 auto;
    text-align: center;
    font-size: 16px;
    line-height: 36px;
    letter-spacing: 0.1em;
    max-width: 516px;
  }
  &__img {
    margin-top: 60px;
  }
}

.coctails__list {
  display: flex;
  align-items: center;
  margin-top: 60px;
  max-height: 400px;
  flex-wrap: wrap;
  overflow-y: auto;
}
</style>
