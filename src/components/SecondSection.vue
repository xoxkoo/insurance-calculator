<template>
	<section id="section-1" v-if="index == 1">
		<div class="date-picker">
			<span class="error" v-if="start == null && showError == true">Prosím zvoľte začiatok poistenia</span>
			<VueDatePicker @internal-model-change="onSelectStart()" v-model="start" placeholder="Začiatok *" auto-apply
				:close-on-auto-apply="true" :enable-time-picker="false" :format="'dd.MM.yyyy'" :disabled-dates="isPastDate">
			</VueDatePicker>

			<span class="error" v-if="end == null && showError == true && data.variant == 'short'">Prosím zvoľte koniec
				poistenia</span>
			<VueDatePicker @internal-model-change="onSelectEnd()" v-if="data.variant == 'short'" v-model="end"
				placeholder="Koniec *" auto-apply :close-on-auto-apply="true" :enable-time-picker="false"
				:format="'dd.MM.yyyy'" :disabled-dates="isPastStart" :disabled="start == null">
			</VueDatePicker>
		</div>

		<h2>Prosím vyberte balík *</h2>
		<span class="error" v-if="pack == '' && showError == true">Prosím vyberte jeden z balíkov</span>
		<div class="select">
			<select v-model="pack" @change="onSelect()" aria-label="Pack Select">
				<option disabled value="">Balík</option>
				<option v-for="(pack, index) in insuranceData[data.variant].packs" :key="index" :value="index">
					{{ pack.name }} ({{ formatPrice(pack.price) }})
				</option>

			</select>
		</div>

		<h2>Počet osôb (1-3) *</h2>
		<input type="number" min="1" max="3" v-model="people" name="people" @input="onPeopleChange()">

		<div class="additionals">
			<h2>Pripoistenia:</h2>

			<label class="control checkbox" v-for="(additional, index) in insuranceData[data.variant].additionals"
				:key="index">
				<input v-model="additionals" type="checkbox" :value="index" @change="onAdditionalsChange()" />
				<span class="control-indicator"></span>
				{{ additional.name }} ({{ percentize(additional.price) }}% prirážka)
			</label>

		</div>

		<div class="row">
			<button class="btn" @click="backStep()">späť</button>
			<button class="btn" @click="calculate()">vypočítať</button>
		</div>

		<CalculatorComponent ref="Calculator" :data="data" />
	</section>
</template>

<script>
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'

import insuranceData from "@/data/data.json";

import CalculatorComponent from './CalculatorComponent.vue'

export default {
	components: { VueDatePicker, CalculatorComponent },
	props: {
		index: {},
		data: {}
	},
	data() {
		return {
			people: 1,
			pack: '',
			start: null,
			end: null,
			additionals: [],
			tmpData: this.data,
			showError: false,
			insuranceData
		}
	},
	methods: {
		percentize(number) {
			return ((number - 1) * 100).toFixed(0)
		},
		formatPrice(number) {
			return new Intl.NumberFormat('de-DE', { style: 'currency', currency: 'EUR' }).format(+number)
		},
		onSelectStart() {
			const tmp = this.data
			tmp.start = this.start
			this.$emit('change-data', tmp)
		},
		onSelectEnd() {
			const tmp = this.data
			tmp.end = this.end
			this.$emit('change-data', tmp)
		},
		onSelect() {
			const tmp = this.data
			tmp.package = this.pack
			this.$emit('change-data', tmp)
		},
		onAdditionalsChange() {
			const tmp = this.data
			tmp.additionals = this.additionals
			this.$emit('change-data', tmp)
		},
		onPeopleChange() {
			const tmp = this.data
			tmp.people = this.people
			this.$emit('change-data', tmp)
		},
		backStep() {
			// this.data.end = null
			let next = this.index - 1
			this.$emit('change-step', next)

			const tmp = this.data
			tmp.end = null
			this.$emit('change-data', tmp)
		},
		isPastDate(date) {
			const currentDate = new Date();
			currentDate.setDate(currentDate.getDate() - 1)
			return date < currentDate;
		},
		isPastStart(date) {
			const dayBefore = new Date(this.start)
			dayBefore.setDate(dayBefore.getDate() - 1);

			return date < dayBefore
		},
		calculate() {
			if (!this.validate()) {
				this.showError = true
				return
			}

			this.showError = false
			this.$refs.Calculator.calculate()
		},
		validate() {
			if (this.data.start == null) {
				return false
			}

			if (this.data.package == null) {
				return false
			}

			return true
		}
	}
}
</script>

<style lang="sass" scoped>

</style>