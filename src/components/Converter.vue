<template>
	<div class="Converter">
		<div class="convert_item">
			<p class="converter_title"> {{ selectedLeft.Name }}</p>
			<div class="converter_content">
				<select 
				class="converter_select" 
				@change="calcResult()"
				v-model="selectedLeft">
					<option 
					v-for="option in data"
					:key="option.ID"
					:value="option">
						{{ option.CharCode }}
					</option>
				</select>

				<input 
				class="converter_input" 
				@input="inputHandler()"
				v-model="leftInput"
				type="text">
			</div>
		</div>
		<button 
		@click="changeBtnHandler()"
		class="converter_btn-item"><SwitchHorizontalIcon class="icon-btn"/>
		</button>
		<div class="convert_item">
			<p class="converter_title"> {{ selectedRight.Name }}</p>
			<div class="converter_content">
				<select 
				class="converter_select"
				@change="calcResult()"
				v-model="selectedRight">
					<option 
					v-for="item in data"
					:key="item.ID"
					:value="item">
						{{ item.CharCode }}
					</option>
				</select>
				<input 
				class="converter_input"
				disabled
				v-model="rightInput"
				type="text">
			</div>
		</div>
	</div>
</template>
<script> 
	import { SwitchHorizontalIcon } from '@heroicons/vue/solid'

	export default{
		components: {
			SwitchHorizontalIcon
		},
		data(){
			return{
				selectedLeft: '',
				leftInput: '',
				selectedRight: '',
				rightInput: ''
			}
		},
		props: ['data'],
		methods: {
			inputHandler(){
				this.leftInput = this.leftInput.replace(/^.*?((\d+\.?|\.)\d{0,4}).*|.+/, '$1');
				this.leftInput = this.leftInput.replace(/^\.$|^(\.)/, '0$1');
				if(this.leftInput){
					this.calcResult()
				}
				if(this.leftInput == '') return this.rightInput = '';
			},
			calcResult(){
				if(this.leftInput == '') return this.rightInput = '';
				const left = this.selectedLeft;
				const right = this.selectedRight;
				let leftRub = ((left.Nominal / left.Value) / left.Nominal) * left.Nominal;
				let rightRub = ((right.Nominal / right.Value) / right.Nominal) * right.Nominal;
				let result = (rightRub / leftRub) * this.leftInput;
				this.rightInput = result.toFixed(4) || result;
			},
			changeBtnHandler(){
				const newSelectedRight = this.selectedLeft;
				const newRightInput = this.leftInput;
				this.selectedLeft = this.selectedRight;
				this.selectedRight = newSelectedRight;
				this.leftInput = this.rightInput;
				this.rightInput = newRightInput;
			}
		},
		created(){
			//default options
			this.selectedLeft = Object.values(this.data)[0];
			this.selectedRight = Object.values(this.data)[1];
			this.leftInput = Object.values(this.data)[0].Nominal;
			this.calcResult();
		}
	}
</script>
<style scoped>
.Converter{
	margin: 40px auto 0 auto;
	display: flex;
	justify-content: space-between;
	align-items: center;
	max-width: 600px;
}
.convert_item{
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
.converter_title{
	font-size: 12px;
	color: rgba(255, 255, 255, .8);
	text-align: left;
	word-wrap: break-word;
}
.converter_content{
	display: flex;
	justify-content: space-between;
	align-items: center;
}
.converter_input{
	border: none;
	background: transparent;
	border-bottom: 1px solid #fff;
	color: #fff;
	margin-left: 5px;
}
.converter_select{
	border: none;
}
.icon-btn{
	color: #fff;
	width: 24px;
	height: 24px;
}
.converter_btn-item{
	width: 40px;
	height: 40px;
	display: flex;
	justify-content: center;
	align-items: center;
	background: rgba(0, 0, 0, .4);
	border: none;
	cursor: pointer;
	border-radius: 50%;
}
.converter_btn-item:active .icon-btn{
	color: #15ff00;
}
.converter_select{
	border: none;
	outline: none;
	color: #fff;
	background: transparent;
	padding: 5px;
	-webkit-appearance: none;
	-moz-appearance: none;
	-ms-appearance: none;
	appearance: none;
	background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='8' height='8' viewBox='0 0 8 8'%3E%3Cpath fill='%23000' fill-rule='evenodd' class='cls-1' d='M8,0L4.141,6.993,0.012,0.156Z'/%3E%3C/svg%3E%0A");
	background-repeat: no-repeat;
	background-position: 36px 10px;
}
option{
	background: rgba(0, 0, 0, .4);
}
</style>