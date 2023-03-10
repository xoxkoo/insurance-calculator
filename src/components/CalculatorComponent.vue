<template>
	<AlertComponent :message="`Celková cena za poistenie je ${finalPrice}.`" :show="show" @hide-alert="show = false" />
</template>

<script>
import insuranceData from "@/data/data.json";
import AlertComponent from './AlertComponent.vue';

export default {
	props: {
		data: {}
	},
	components: { AlertComponent },
	data() {
		return {
			finalPrice: 0,
			show: false
		}
	},
	methods: {
		formatPrice(number) {
			return new Intl.NumberFormat('de-DE', { style: 'currency', currency: 'EUR' }).format(+number)
		},
		calculate() {

			const variant = insuranceData[this.data.variant]

			this.finalPrice = +variant.packs[this.data.package].price

			this.finalPrice = this.data.additionals.reduce((a, additional) => {

				return a * variant.additionals[additional].price
			}, this.finalPrice)

			this.finalPrice *= this.data.people

			if (this.data.variant === 'short') {
				this.finalPrice *= this.calculateDays()
			}
			this.finalPrice = this.formatPrice(this.finalPrice)

			this.show = true

		},
		calculateDays() {
			// One day Time in ms (milliseconds)
			const one_day = 1000 * 60 * 60 * 24

			const start = this.data.start
			const end = this.data.end

			if (start.getMonth() == 11 && start.getdate() > 25)
				end.setFullYear(end.getFullYear() + 1)

			// To Calculate the result in milliseconds and then converting into days
			const Result = Math.round(end.getTime()
				- start.getTime()) / (one_day);

			// To remove the decimals from the (Result) resulting days value
			// add one day
			return +Result.toFixed(0) + 1;
		}
	}
}
</script>

<style></style>