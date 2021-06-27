<template>
	<div class="list">
		<ul class="list_list">
			<Valute 
				v-for="valute of data"
				v-bind:valute="valute[1]"
				v-bind:key="valute[1].ID"
			/>
		</ul>
	</div>
</template>

<script>
	import Valute from '@/components/Valute'

	export default{
		data(){
			return{
				searchValue: ''
			}
		},
		components: {
			Valute
		},
		props: ['data'],
		methods: {

		}
	}
</script>

<style scoped>
.list{
	width: inherit;
	display: flex;
	flex-direction: column;
}
.list_list{
	margin: 20px 0;
	display: flex;
	flex-wrap: wrap;
	justify-content: center;
	align-items: center;
}

</style>