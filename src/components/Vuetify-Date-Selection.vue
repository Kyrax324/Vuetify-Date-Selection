<template>
<v-card flat :dark="dark" :light="light">
		<v-row no-gutters :class=" (!!dark ? '' : 'primary') " class="text-center white--text pa-3" align="center" justify="space-around">
			<v-col>
				<div v-if="start_date" class="text-left px-4">
					<div class="body-1">{{ $refs.zid_picker_1.formatters.year(start_date) }}</div>
					<div class="headline font-weight-bold">{{ $refs.zid_picker_1.formatters.titleDate(start_date) }}</div>
				</div>
				<div v-else>
					<div class="px-4 py-2 title">{{ startText }}</div>
				</div>
			</v-col>
			<template v-if="range">
				<v-col cols=auto>
					<slot name="divider">
						<span>to</span>
					</slot>
				</v-col>
				<v-col>
					<div v-if="end_date" class="text-left px-4">
						<div class="body-1">{{ $refs.zid_picker_1.formatters.year(end_date) }}</div>
						<div class="headline font-weight-bold">{{ $refs.zid_picker_1.formatters.titleDate(end_date) }}</div>
					</div>
					<div v-else>
						<div class="px-4 py-2 title">{{ endText }}</div>
					</div>
				</v-col>
			</template>
		</v-row>
		<v-row no-gutters justify="space-around">
			<v-col>
				<v-date-picker 
					ref="zid_picker_1" 
					v-model="start_date"
					scrollable no-title
					width="auto"
					:dark="dark" :light="light"
					:color="color" :locale="locale" :first-day-of-week="firstDayOfWeek"
					:events="[].concat([ end_date ], date_range.range)" :event-color="eventColor"
					@input="input_start()"
				></v-date-picker>
			</v-col>
			<template v-if="range">
				<v-divider vertical class="mx-1 my-3"></v-divider>
				<v-col>
					<v-date-picker 
						ref="zid_picker_2" 
						v-model="end_date"
						:min="end_min" 
						srollable no-title
						width="auto"
						:dark="dark" :light="light"
						:color="color" :locale="locale" :first-day-of-week="firstDayOfWeek"
						:events="[].concat([ start_date ], date_range.range)" :event-color="eventColor"
						@input="input_end()"
					></v-date-picker>
				</v-col>
			</template>
		</v-row>
		<v-card-actions>
			<slot></slot>
		</v-card-actions>
</v-card>
</template>

<script>
export default {
	components: { 
	},
	props:{
		value: {
			required: true,
		},
		range:{
			default: false
		},
		color:{},
		eventColor:{},
		locale:{},
		headerColor: {},
		dark:{
			type: Boolean,
			default : false
		},
		light:{
			type: Boolean,
			default : false
		},
		startText:{
			type: String,
			default : 'Start'
		},
		endText:{
			type: String,
			default : 'End'
		},
		firstDayOfWeek:{
			default: 0
		}
	},
	data () {
		return {
			start_date: null,
			end_date: null,
			end_min : null,
		}
	},
	created(){

	},
	watch:{
		value:{
			handler(val){
				if(Array.isArray(val) && val.length <= 2){
					this.$nextTick(()=>{
						[this.start_date, this.end_date] = val
						this.end_min = this.start_date
					})
				}
			},
            immediate : true,
		},
		range:{
			handler(val){
				if(!val){
					this.end_date = null
					this.$emit('input',[this.start_date])
				}
			}
		}
	},
	computed:{
		date_range(){
			let startDate = this.start_date
			let stopDate = this.end_date

			var dateArray = new Array();
			var currentDate = this.addDays(startDate,1)
			while (currentDate < stopDate) {
				dateArray.push(new Date (currentDate).toISOString().slice(0,10));
				currentDate = this.addDays(currentDate,1);
			}
			let date_range = {
				start : this.start_date,
				end : this.end_date,
				range : dateArray
			}
			return date_range;
		}
	},
	methods: {
		input_start(){
			this.end_min = this.start_date
			this.$emit('input',[this.start_date])
			if(this.start_date > this.end_date){
				this.end_date = null
			}
		},
		input_end(){
			this.$emit('input',[this.start_date, this.end_date] )
		},
		addDays(currentDate,days){
			var date = new Date(currentDate);
			date.setDate(date.getDate() + days);
			return date.toISOString().slice(0,10);
		}
	}
}
</script>

<style scoped>
	.auto-width{
		width: 50% !important;
	}
</style>