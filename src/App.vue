<script>
import BooksOverview from '@/components/BooksOverview.vue'
// import CounterBasic from '@/components/CounterBasic.vue'
// import SeriesOverview from '@/components/SeriesOverview.vue'

export default {
	name: 'App',
	components: {
		BooksOverview
		// SeriesOverview,
		// CounterBasic
	},
	data() {
		return {}
	}
}
</script>

<template>
	<BooksOverview />
	<!-- <CounterBasic /> -->
	<!-- <SeriesOverview /> -->
</template>

<style scoped></style>
