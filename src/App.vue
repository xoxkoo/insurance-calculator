<template>
	<main>
		<h1>Vitajte!</h1>
		<HomeSection :index="index" :data="data" @change-data="changeData($event)" @change-step="changeStep($event)" />
		<SecondSection :index="index" :data="data" @change-data="changeData($event)" @change-step="changeStep($event)" />

	</main>
</template>

<script>
import HomeSection from '@/components/HomeSection.vue'
import SecondSection from '@/components/SecondSection.vue'
import '@/assets/css/wtf-forms.css'


export default {
	name: 'App',
	data() {
		return {
			data: { people: 1, additionals: [] },
			index: 0,
			validate: true
		}
	},
	components: {
		HomeSection, SecondSection
	},
	methods: {
		changeStep(step) {
			this.index = step
		},
		changeData(data) {
			this.data = data
		}
	}
}
</script>

<style lang="sass">
	@import '@/assets/sass/base.sass'
</style>
