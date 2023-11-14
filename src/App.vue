<template>
	<HelloWorld msg="Welcome to Your Vue.js App" />
	<BooksOverview />
	<SeriesOverview />
</template>

<script>
import SeriesOverview from '@/components/SeriesOverview.vue'
import BooksOverview from '@/components/BooksOverview.vue'
export default {
	name: 'App',
	components: { SeriesOverview, BooksOverview }
}
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}

body {
	background: black;
	color: white;
	padding: 0 1rem;
	margin: 0;
	/* letter-spacing: -0.05em; */
	font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
	text-align: left;
}

a {
	color: lightblue;
}

img {
	max-width: 75px;
	object-fit: cover;
	display: inline-block;
	float: right;
	margin-top: 1em;
}

h4 {
	margin-bottom: 0;
}

ul {
	margin-left: 0;
	padding-left: 0;
	margin-bottom: 0;
	margin-top: 0;
	list-style-type: none;
}

li {
	padding: 0.5em 0;
	margin: 0;
	clear: both;
}
</style>
