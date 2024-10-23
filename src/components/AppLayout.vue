<template>
  <div class="root">
    <div :style="`background-image: url(${imgUrl})`" class="root__img"></div>
    <div class="root__main">
      <div class="root__btns">
        <el-button
          v-if="isBackBtnVisible"
          @click="goBack"
          type="primary"
          :icon="Back"
          circle
          class="root__back-btn"
        />
        <el-button class="main__btn" @click="goForCocktailRandom"
          >Get random cocktail</el-button
        >
      </div>
      <slot></slot>
    </div>
  </div>
</template>
<script setup>
import { computed } from "vue";
import { useRoute, useRouter } from "vue-router";
import { Back } from "@element-plus/icons-vue";
import { ROUTES_PATHS } from "@/constants";

const props = defineProps({
  imgUrl: {
    type: String,
    required: true,
  },
  backFunc: {
    type: Function,
  },
  isBackBtnVisible: {
    type: Boolean,
    default: true,
  },
});

const route = useRoute();
const router = useRouter();

const routeName = computed(() => route.name);

function goForCocktailRandom() {
  router.push(ROUTES_PATHS.COCKTAIL_RANDOM);

  if (routeName.value === ROUTES_PATHS.COCKTAIL_RANDOM) {
    router.go();
  }
}

function goBack() {
  props.backFunc ? props.backFunc() : router.go(-1);
}
</script>
<style lang="scss" scoped>
@import "../assets/styles/main";

.root {
  display: flex;
  min-height: 100vh;

  &__img {
    width: 50%;
    background-repeat: no-repeat;
    background-position: 50% 50%;
    background-size: cover;
  }

  &__main {
    width: 50%;
    position: relative;
    padding: 32px 40px;
  }
}

.root__btns {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.main__btn {
  padding: 8px 16px;
  position: absolute;
  top: 32px;
  right: 40px;
  background-color: $accent;
  color: $text-c;
  font-size: 16px;
  font-weight: 400;
  border-width: 0;
  transition: all 0.2s linear;
  font-family: "Raleway", "Arial", sans-serif;

  &:hover,
  &:active {
    background-color: $accent-hov;
  }
}

.root__back-btn {
  background-color: transparent;
  border-color: #fff;

  &:hover {
    border-color: $accent;
  }
}
</style>
