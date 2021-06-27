<template>
	<li class="valute">
		<p class="valute_title">{{ valute.Name }}</p>
		<div class="valute_content">
			<div class="valute_info">
				<span class="valute_nominal">{{ nominal }}</span>
				<span class="valute_code">{{ code }}</span>
				<button 
				class="valute_btn" 
				@click="valuteChangeHandler()">
				<SwitchHorizontalIcon class="icon-btn"/>
				</button>
				<span class="valute_value">{{ value }}</span>
				<span>{{ mainValuteValue }}</span>
			</div>
			<span 
			v-bind:class="[differenceGreen ? 'green' : 'red']"
			class="valute-difference">
			<ArrowNarrowUpIcon v-if="differenceGreen" class="icon-arrow green"/>
			<ArrowNarrowDownIcon v-if="!differenceGreen" class="icon-arrow red"/>
			{{ difference }}
			</span>
		</div>
	</li>
</template>
<script>
	import { SwitchHorizontalIcon, ArrowNarrowUpIcon, ArrowNarrowDownIcon } from '@heroicons/vue/solid'

	export default {
		components: {
			SwitchHorizontalIcon,
			ArrowNarrowUpIcon,
			ArrowNarrowDownIcon
		},
		data(){
			return{
				difference: null,
				differenceGreen: null,
				code: '',
				nominal: '',
				value: '',
				mainValuteValue: 'RUB'
			}
		},
		methods:{
			valuteChangeHandler(){
				if(this.code == this.valute.CharCode){
					const cloneCode = this.code
					this.code = this.mainValuteValue
					this.mainValuteValue = cloneCode
					this.calcValue(this.nominal, this.value)
				}
				else{
					const cloneMain = this.mainValuteValue
					this.mainValuteValue = this.code
					this.code = cloneMain
					this.value = this.valute.Value
				}
			},
			calcValue(nom, val){
				let result = (nom / val) * nom;
				this.value = result.toFixed(4);
			}
		},
		created() {
			//default options
			const result = this.valute.Value - this.valute.Previous;
			result > 0 ? this.differenceGreen = true : this.differenceGreen = false;
			result > 0 ? this.difference = `+${result.toFixed(4)}` : this.difference = result.toFixed(4);

			this.code = this.valute.CharCode
			this.nominal = this.valute.Nominal
			this.value = this.valute.Value.toFixed(4)
		},
		props: ['valute']
	}
</script>

<style scoped>
.valute{
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	padding: 6px 10px;
	margin: 15px;
	background: rgba(0, 0, 0, .4);
	width: 330px;
	border-radius: 6px;
	cursor: default;
}
.valute:hover{
	background: rgba(0, 0, 0, .3);
}
.valute_title{
	font-size: 12px;
	color: rgba(255, 255, 255, .8);
	text-align: left;
	word-wrap: break-word;
}
.green{
	color: #15ff00;
}
.red{
	color: #ff0808;
}
.valute_content{
	display: flex;
	justify-content: space-between;
}
.valute_info{
	display: flex;
	color: #fff;
}
.valute-difference{
	display: flex;
	align-items: center;
	padding-left: 4px;
}
.valute_nominal{
	margin-right: 5px;
}
.valute_value{
	margin-right: 5px;
}
.valute_btn{
	display: flex;
	justify-content: center;
	align-items: center;
	background: none;
	border: none;
	cursor: pointer;
}
.icon-btn{
	width: 18px;
	height: 16px;
	padding: 0;
	color: #fff;
}
.icon-btn:active{
	color: #15ff00;
}
.icon-arrow{
	width: 16px;
	height: 16px;
}

</style>