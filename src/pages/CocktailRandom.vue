<template>
  <div v-if="cocktail" class="wrap">
    <AppLayout :imgUrl="cocktail.strDrinkThumb">
      <div class="info">
        <div class="info__inner">
          <h1 class="info__title">{{ cocktail.strDrink }}</h1>

          <div class="info__text">
            <div class="info__slider">
              <swiper :slides-per-view="3" :space-between="50" class="slider">
                <swiper-slide
                  v-for="(ingredient, key) in ingredients"
                  :key="key"
                >
                  <img
                    :src="`${INGREDIENT_IMG}${ingredient}-Small.png`"
                    alt=""
                  />
                  {{ ingredient }}
                </swiper-slide>
              </swiper>
            </div>

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
import AppLayout from "../components/AppLayout.vue";
import { COCKTAIL_RANDOM, INGREDIENT_IMG } from "@/constants";
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";

const cocktail = ref(null);

const ingredients = computed(() => {
  const ingredients = [];

  for (let i = 1; i <= 15; i++) {
    if (!cocktail.value[`strIngredient${i}`]) break;

    const ingredient = cocktail.value[`strIngredient${i}`];

    ingredients.push(ingredient);
  }

  return ingredients;
});

async function getCocktail() {
  const data = await axios.get(COCKTAIL_RANDOM);
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
  &__slider {
    margin-bottom: 50px;
    .slider {
      width: 586px;
    }
    .swiper-slide {
      display: flex;
      flex-direction: column;
      align-items: center;
      color: $text-c;
      font-size: 16px;
      font-weight: 400;
      line-height: 22px;
      letter-spacing: 1.6px;
      text-align: center;
      img {
        margin-bottom: 20px;
      }
    }
  }
}
</style>
