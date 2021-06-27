<template>
	<div 
	v-if="data != ''" 
	class="Daily">
		<div class="menu">
			<button 
			v-on:click="showConverter()"
			class="menu_btn btn--margin">Конвертер</button>
			<button 
			v-on:click="showList()"
			class="menu_btn">Список валют</button>
		</div>
		<div 
		v-if="list"
		class="list-search">
			<input 
			v-model="searchValue"
			placeholder="Search valute"
			class="list-search_input" 
			type="text">
		</div>
		<List 
		v-if="list"
		v-bind:data="filteredData"/>
		<Converter 
		v-bind:data="this.data"
		v-if="converter"
		/>
	</div>
</template>

<script>
	import axios from 'axios'
	import List from '@/components/List'
	import Converter from '@/components/Converter'

	export default {
		components: {
			Converter,
			List
		},
		methods: {
			showList(){
				this.list = !this.list;
				this.converter = false;
			},
			showConverter(){
				this.converter = !this.converter;
				this.list = false;
			}
		},
		computed: {
			filteredData(){
				const value = this.searchValue.toUpperCase();
				if(value.trim()){
					return Object.entries(this.data).filter(item => {
						//Фильтрация по символам
						let key = item[0].split('');// разбиваем на массив ключ
						key.splice(value.length, key.length - value.length); //удаляем все остальные символы
						let name = item[1].Name.toUpperCase().split('');// делаем тоже самое с названием
						name.splice(value.length, name.length - value.length);

						if(key.join('') == value){//преобразуем в строку и сравниваем 
							return item;
						}
						else if(name.join('') == value){
							return item;
						}

						//Фильтрация по содержимому
						// if(item[0].includes(value)){
						// 	return item;
						// }
						// else if(item[1].Name.toUpperCase().includes(value)){
						// 	return item;
						// }
					});
				}
				return Object.entries(this.data);
			}
		},
		data(){
			return {
				data: '',
				converter: false,
				list: false,
				searchValue: ''
			}
		},
		mounted() {
			axios.get('https://www.cbr-xml-daily.ru/daily_json.js')
			.then(res => this.data = res.data.Valute);
		}
	}
</script>

<style>
.Daily{
	width: 100%;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}
.list-search{
	width: 100%;
	margin-top: 40px;
}
.list-search_input{
	padding: 8px;
	font-size: 16px;
	font-weight: 600;
	border-radius: 8px;
}
.menu{
	display: flex;
	margin-top: 40px;
}
.menu_btn{
	background: firebrick;
	color: #fff;
	padding: 5px 10px;
	border: none;
	cursor: pointer;
	border-radius: 5px;
}
.btn--margin{
	margin-right: 20px;
}
	
</style>