<script setup>
import { ref } from 'vue'
import AppLayout from '@/components/AppLayout.vue'
import CocktailThumb from '@/components/CocktailThumb.vue'
import { useRootStore } from '@/stores/root'
import { storeToRefs } from 'pinia'
const rootStore = useRootStore()
rootStore.getIngredients()

const { ingredients, ingredient, cocktails } = storeToRefs(rootStore)

function getCocktails() {
	rootStore.getCocktails(rootStore.ingredient)
}

function removeIngredient() {
	rootStore.setIngredient(null)
	console.log("Ingredient removed!");
}

</script>

<template>
	<AppLayout imgUrl="/src/assets/img/bg-1.jpg" :backFunc="removeIngredient" :is-back-button-visible="!!ingredient">
		<div class="wrapper">
			<div v-if="!ingredient || !cocktails" class="info">
				<div class="title">Choose your drink</div>
				<div class="line"></div>
				<div class="select-wrapper">
					<el-select
						class="select"
						filterable
						allow-create
						v-model="rootStore.ingredient"
						placeholder="Choose main ingredient"
						style="width: 240px"
						@change="getCocktails"
					>
						<el-option
							v-for="item in ingredients"
							:key="item.strIngredient1"
							:label="item.strIngredient1"
							:value="item.strIngredient1"
						/>
					</el-select>
				</div>
				<div class="text">
					Try our delicious cocktail recipes for every occasion. Find delicious
					cocktail recipes by ingredient through our cocktail generator.
				</div>
				<img src="/src/assets/img/cocktails.png" alt="Cocktails" class="img" />
			</div>
			<div v-else class="info">
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
		</div>
	</AppLayout>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main'

.wrapper
	display: flex
	justify-content: center
	align-items: center

.info
	padding: 80px 0
	text-align: center

.select-wrapper
	padding-top: 50px

.select
	width: 220px

.text
	max-width: 516px
	margin: 0 auto
	padding-top: 50px
	line-height: 36px
	letter-spacing: 0.1em
	color: $textMuted

.img
	width: 345px
	height: 120px
	margin-top: 60px

.cocktails
	display: flex
	align-items: center
	margin-top: 60px
	flex-wrap: wrap
	overflow-y: auto
	max-height: 400px
</style>
