<template>
	<article class="alert" v-if="show">
		<h2>
			{{ message }}
		</h2>
		<button class="btn btn-secondary" @click="hide()">potvrdiť</button>
	</article>
</template>

<script>
export default {

	props: {
		message: {},
		show: {}
	},
	data() {
		return {
		}
	},
	methods: {
		hide() {
			this.$emit('hide-alert', true)
		}
	}

}
</script>

<style lang="scss" scoped>

</style>