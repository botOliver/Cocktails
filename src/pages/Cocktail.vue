<script setup>
import { useRoute } from 'vue-router'
import { computed } from 'vue'
import { ref } from 'vue'
import { COCKTAIL_BY_ID } from '@/constants'
import axios from 'axios'
import AppLayout from '@/components/AppLayout.vue'
const route = useRoute()
const cocktailID = computed(() => route.path.split('/').pop())

const cocktail = ref(null)
async function getCocktail() {
	const data = await axios.get(`${COCKTAIL_BY_ID}${cocktailID.value}`)
	cocktail.value = data?.data?.drinks[0]
}

const ingredients = computed(() => {
	const ingredients = []

	for (let i = 1; i <= 15; i++) {
		if (!cocktail.value[`strIngredient${i}`]) break

		const ingredient = {}
		ingredient.name = cocktail.value[`strIngredient${i}`]
		ingredient.measure = cocktail.value[`strMeasure${i}`]
		ingredients.push(ingredient)
	}
	return ingredients
})

getCocktail()
</script>

<template>
	<div v-if="cocktail" class="wrap">
		<AppLayout :imgUrl="cocktail.strDrinkThumb">
			<div class="cocktail">
				<div class="wrapper">
					<div class="info">
						<div class="title">{{ cocktail.strDrink }}</div>
						<div class="line"></div>
						<div class="list">
							<div
								v-for="(item, key) in ingredients"
								:key="key"
								class="list-item"
							>
								{{ item.name }}
								<template v-if="item.measure">
									|
									{{ item.measure }}
								</template>
							</div>
						</div>
						<div class="instructions">
							{{ cocktail.strInstructions }}
							</div>
						<div class="test"></div>
					</div>
				</div>
			</div>
		</AppLayout>
	</div>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main'
</style>
