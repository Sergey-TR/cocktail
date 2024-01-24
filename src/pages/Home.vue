<script setup>
import AppLayout from "@/components/AppLayout.vue";
import CocktailThumb from "@/components/CocktailThumb.vue";
import { useRootStore } from "@/stores/root";
import { storeToRefs } from "pinia";
import { ref } from "vue";

const rootStore = useRootStore();
rootStore.getIngredients();

const { ingredients, ingredient, cocktails } = storeToRefs(rootStore);

function getCocktails() {
  rootStore.getCocktails(rootStore.ingredient);
}

function removeIngredient() {
  rootStore.setIngredient(null);
}
</script>

<template>
  <AppLayout
    imgUrl="/src/assets/img/bg-1.jpg"
    :backFunc="removeIngredient"
    :is-back-btn-visible="ingredient"
  >
    <div class="wrapper">
      <div v-if="cocktails.length != 0 && ingredient" class="info">
        <div class="title">COCKTAILS WITH {{ ingredient }}</div>
        <div class="line"></div>
        <div class="cocktails">
          <CocktailThumb
            v-for="cocktail in cocktails"
            :key="cocktail.idDrink"
            :cocktail="cocktail"
          />
        </div>
      </div>
      <div v-else class="info">
        <div class="title">Choose your drink</div>
        <div class="line"></div>
        <div class="select-wrapper">
          <select
            class="select"
            v-model="rootStore.ingredient"
            filterable
            @change="getCocktails"
          >
            <option value="null" disabled selected hidden class="selected">
              Choose main ingredient
            </option>
            <option
              class="option"
              v-for="item in ingredients"
              :key="item.strIngredient1"
              :label="item.strIngredient1"
              :value="item.strIngredient1"
            ></option>
          </select>
        </div>
        <div class="text">
          Try our delicious cocktail recipes for every occasion. Find delicious
          cocktail recipes by ingredient through our cocktail generator.
        </div>
        <img src="/src/assets/img/cocktails.png" alt="Cocktails" class="img" />
      </div>
    </div>
  </AppLayout>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main'

.select-wrapper
  margin-top: 50px
.select
  border: 1px solid #c4c4c4
  background: none
  outline: none
  width: 250px
  height: 40px
  border-radius: 7px
  padding: 0px 11px
  color: $textMuted
  font-family: Raleway
  font-size: 16px
  font-weight: 500
  line-height: 19px
  letter-spacing: 0em
  --webkit-appearance: none
  --moz-appearance: none
  --ms-appearance: none
  appearance: none
  background-image: url("data:image/svg+xml,%3Csvg width='10' height='5' viewBox='0 0 10 5' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M0 0L5 5L10 0H0Z' fill='white' fill-opacity='0.54'/%3E%3C/svg%3E%0A")
  background-repeat: no-repeat
  background-position-x: right
  background-position-y: center
  background-origin: content-box

.selected
  font-family: Raleway
  font-size: 16px
  font-weight: 500
  line-height: 19px
  letter-spacing: 0em
  text-align: center

.option
  color: $textMuted
  font-size: 16px
  background: #141414

.text
  max-width: 516px
  margin: 0 auto
  padding-top: 50px
  line-height: 36px
  letter-spacing: 0.1em
  color: $textMuted

.img
  margin-top: 60px

.cocktails
  display: flex
  flex-wrap: wrap
  align-items: center
  max-height: 400px
  overflow-y: auto
  margin-top: 60px
</style>