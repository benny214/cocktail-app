<template>
  <div v-if="cocktail" class="wrap">
    <AppLayout :imgUrl="cocktail.strDrinkThumb">
      <div class="info">
        <div class="info__inner">
          <h1 class="info__title">{{ cocktail.strDrink }}</h1>

          <div class="info__text">
            <ul class="info__list">
              <li
                v-for="(item, key) in ingredients"
                :key="key"
                class="info__item"
              >
                {{ item.name }}
                <template v-if="item.measure">
                  |
                  {{ item.measure }}
                </template>
              </li>
            </ul>
            <div class="info__instruction">
              {{ cocktail.strInstructions }}
            </div>
          </div>
        </div>
      </div>
    </AppLayout>
  </div>
</template>
<script setup>
import axios from "axios";
import { ref, computed } from "vue";
import { useRoute } from "vue-router";
import AppLayout from "../components/AppLayout.vue";
import { COCKTAIL_BY_ID } from "@/constants";

const route = useRoute();

const cocktail = ref(null);
const cocktailId = computed(() => route.path.split("/").pop());

const ingredients = computed(() => {
  const ingredients = [];

  for (let i = 1; i <= 15; i++) {
    if (!cocktail.value[`strIngredient${i}`]) break;

    const ingredient = {};
    ingredient.name = cocktail.value[`strIngredient${i}`];
    ingredient.measure = cocktail.value[`strMeasure${i}`];

    ingredients.push(ingredient);
  }

  return ingredients;
});

async function getCocktail() {
  const data = await axios.get(`${COCKTAIL_BY_ID}${cocktailId.value}`);
  cocktail.value = data?.data?.drinks[0];
}

getCocktail();
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
  &__title {
    margin-bottom: 50px;
  }
  &__text {
    color: $text-muted;
    margin: 0 auto;
    text-align: left;
    font-size: 16px;
    line-height: 36px;
    letter-spacing: 0.1em;
    max-width: 516px;
  }
  &__list {
    list-style: none;
    margin-bottom: 80px;
    width: 560px;
  }
  &__item {
    display: flex;
    align-items: center;
    font-size: 18px;
    font-weight: 400;
    line-height: 27px;
    letter-spacing: 1.8px;
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
    &::before {
      content: "";
      flex-shrink: 0;
      width: 17px;
      height: 17px;
      margin-right: 23px;
      background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='23' height='23' fill='none'%3E%3Cg fill='%23FF0F82' clip-path='url(%23a)'%3E%3Cpath d='M9.683 5.527c.695-.468 1.252-.887 1.85-1.238 1.02-.6 2.088-.728 3.225-.282 2.06.822 3.175 2.336 3.455 4.51.154 1.196-.347 2.205-.998 3.132-.681.977-1.452 1.889-2.162 2.85-.98 1.314-1.658 2.757-1.835 4.415-.036.326-.238.634-.339.886-.46.17-.775.156-1.023-.19-.145-.195-.325-.366-.477-.556-.666-.828-1.491-1.47-2.363-2.063-1.304-.884-2.653-1.701-3.883-2.69a8.808 8.808 0 0 1-2.422-3.026c-.646-1.333-.749-2.752-.16-4.184.676-1.646 3.007-3.322 5.398-2.62.489.143.922.485 1.37.75.13.09.253.193.364.306Zm2.181 11.996c.067-.083.122-.175.164-.272.458-1.693 1.358-3.136 2.463-4.478a31.494 31.494 0 0 0 1.878-2.528c.915-1.35.668-2.572-.24-3.795a4.511 4.511 0 0 0-1.005-.916c-.73-.526-1.537-.623-2.395-.325-.88.308-1.634.775-2.202 1.54-.318.425-.786.436-1.183.09a1.898 1.898 0 0 1-.283-.309c-.514-.697-1.242-.861-2.047-.837-.705.022-1.39.24-1.978.63a2.351 2.351 0 0 0-.731.731c-.674 1.148-.84 2.372-.347 3.632a6.189 6.189 0 0 0 2.152 2.797c.681.5 1.372.97 2.073 1.434 1.17.771 2.364 1.503 3.394 2.467.089.06.186.106.287.139Z'/%3E%3Cpath d='M11.864 17.523a1.151 1.151 0 0 1-.284-.141c-1.03-.965-2.224-1.697-3.394-2.468a54.288 54.288 0 0 1-2.076-1.43 6.19 6.19 0 0 1-2.154-2.795c-.492-1.256-.327-2.483.346-3.631.187-.295.437-.545.731-.73a3.788 3.788 0 0 1 1.98-.635c.806-.024 1.537.138 2.048.837.082.114.177.217.283.31.397.345.865.334 1.183-.091.568-.765 1.325-1.234 2.202-1.54.858-.298 1.664-.2 2.395.326.38.252.718.56 1.004.915.909 1.223 1.152 2.437.241 3.795-.585.87-1.211 1.72-1.878 2.529-1.105 1.341-2.007 2.78-2.463 4.477-.042.097-.097.189-.164.272Z'/%3E%3C/g%3E%3Cdefs%3E%3CclipPath id='a'%3E%3Cpath fill='%23fff' d='M0 6.944 15.49 0l6.931 15.517-15.49 6.944z'/%3E%3C/clipPath%3E%3C/defs%3E%3C/svg%3E");
      background-size: cover;
      background-repeat: no-repeat;
    }
  }
}
</style>
