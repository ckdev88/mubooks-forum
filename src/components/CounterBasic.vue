<script>
export default {
	name: 'CounterBasic',
	data: () => ({
		count: 3,
		incrementAmount: 1,
		message: 'quer massage??',

		listOfNumbers: [
			{ id: 1, num: 1, name: 1, list: [1, 2, 3, 5, 8] },
			{ id: 2, num: 3, name: 3, list: [1, 2, 3] },
			{ id: 3, num: 4, name: 4, list: [9, 2, 3] },
			{ id: 4, num: 6, name: 6, list: [1, 2, 3] },
			{ id: 5, num: 8, name: 8, list: [1, 2, 3] }
		]
	}),
	computed: {
		displayTitle() {
			if (this.count > 20) {
				return 'Counter standard - Very Long'
			}
			return 'Counter standard'
		}
	},
	methods: {
		decreaseNum(index) {
			this.listOfNumbers[index].num--
		},
		decreaseNum2(e) {
			console.log('e:', e)
			this.count -= this.incrementAmount
		},
		incrementCount() {
			this.count += this.incrementAmount
		}
	}
}
</script>
<template>
	<div>
		<h2>{{ displayTitle }}</h2>
		<p>
			<label>Stepping by: {{ incrementAmount }} </label>&nbsp;
			<input type="number" v-model="incrementAmount" />
		</p>
		<p>
			<button @click="incrementCount">+</button>&nbsp;
			<button @click="decreaseNum2">-</button>
		</p>
		<p :data-increment-by="incrementAmount">Count: {{ count }} &nbsp;</p>

		<hr />

		<p v-if="message.length % 2 === 0">even: {{ message }}</p>
		<p v-else>odd: {{ message }}</p>
		<ul>
			<li v-for="(number, index) in listOfNumbers" :key="index">
				{{ number.name }} - {{ number.num }}
				<button @click="number.num++">plus</button>
				<button @click="decreaseNum(index)">decrease</button>
				<!-- <button @click="decreaseNum2(listOfNumbers[index].num)">decrease list</button> -->
				<ul>
					<li v-for="(item, index) in number.list" :key="index">{{ item }}</li>
				</ul>
			</li>
		</ul>
	</div>
</template>

<style></style>
