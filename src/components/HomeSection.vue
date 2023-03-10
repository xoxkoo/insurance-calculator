<template>
	<section id="section-1" v-if="index == 0">
		<h2>Prosím vyberte variant</h2>
		<span class="error" v-if="variant == '' && showError == true">Prosím zvoľte jeden z variantov</span>
		<div class="select">
			<select v-model="variant" @change="onSelect()" aria-label="Variant select" required>
				<option disabled value="">Variant</option>
				<option value="short">Krátkodobé poistenie</option>
				<option value="year">Celoročné poistenie</option>
			</select>
		</div>

		<div class="row">
			<button class="btn center" @click="nextStep()">pokračovať</button>
		</div>

	</section>
</template>

<script>
export default {
	props: {
		index: {},
		data: {}
	},
	data() {
		return {
			variant: '',
			showError: false
		}
	},
	methods: {
		onSelect() {
			const tmp = this.data
			tmp.variant = this.variant
			this.$emit('change-data', tmp)
		},
		nextStep() {
			if (this.variant == '') {
				this.showError = true
				return
			}

			this.showError = false

			let next = this.index + 1
			this.$emit('change-step', next)
		}
	}
}
</script>

<style lang="sass" scoped>

</style>