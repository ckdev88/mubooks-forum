<template>
	<div>
		<h2>Books</h2>
		<ul>
			<li v-for="(book, index) in booklist" :key="index">
				<h3>{{ book.title }} ({{ book.releaseYear }})</h3>
				pages: {{ book.pages }}, chapters:
				{{ book.chapters }}
				<button @click="addFavoriteBook(book.title)" :disabled="favoriteBooks.includes(book.title)">
					add book to favorites
				</button>
				<button v-if="favoriteBooks.includes(book.title)" @click="removeFavoriteBook(book.title)">
					remove from favorites
					{{ book.title }}
				</button>

				<br />Genre:
				<span v-for="(genre, index) in book.genres" :key="index"
					>{{ genre }}
					<span v-if="index < book.genres.length - 1">, </span>
				</span>

				<br />
				Topics:
				<span v-for="(topic, index) in book.topics" :key="index">
					<button @click="addTopic(topic)">{{ topic }}</button>
					<span v-if="index < book.topics.length - 1">, </span>
				</span>

				<br />
				<br />
			</li>
		</ul>
		<h4>add book</h4>
		<form @submit.prevent="addToBooklist">
			<label>Title</label>
			<input type="text" v-model="abTitle" /> {{ abTitle }}<br />
			<label>Year</label>
			<input type="number" v-model="abReleaseYear" /> {{ abReleaseYear }}<br />
			<label>Pages</label>
			<input type="number" v-model="abPages" />{{ abPages }}<br />
			<label>Chapters</label>
			<input type="number" v-model="abChapters" />{{ abChapters }}<br />
			<label>Genres</label>
			<input type="text" v-model="abGenres" />{{ abGenres }}<br />
			<label>Topics</label>
			<input type="text" v-model="abTopics" />{{ abTopics }}<br />
			<button>Add book</button>
		</form>
		<br />
		<button v-if="!showstatsnstuff" @click="showstatsnstuff = !showstatsnstuff">
			Show stats n stuff
		</button>
		<div id="statsnstuff" v-if="showstatsnstuff" @click="showstatsnstuff = !showstatsnstuff">
			<button>Hide stats n stuff</button>
			<h3>Romances</h3>
			<p v-if="genreRomanceBooks.length === 0">No romance books available</p>
			<ul v-else>
				<li v-for="(book, index) in genreRomanceBooks" :key="index">
					{{ book.title }} ({{ book.releaseYear }})
				</li>
			</ul>
			<hr />
			<div v-if="genreDarkRomanceBooks.length > 0">
				<h3>Dark Romances</h3>
				<ul>
					<li v-for="(book, index) in genreDarkRomanceBooks" :key="index">
						{{ book.title }} ({{ book.releaseYear }})
					</li>
				</ul>
			</div>

			<h3>My loved topics</h3>
			<p v-if="favoriteTopics.length === 0">No topics added yet</p>
			<ul>
				<li v-for="(topic, index) in favoriteTopics" :key="index">{{ topic }}</li>
			</ul>

			<h3>Statistics</h3>
			<p>
				Books with more than 150 pages: {{ amountBigBooks }}<br />
				Books released this decade: {{ amountRecentBooks }}<br />
				Books with small chapters: {{ amountBooksSmallChapters }}<br />
				Books sold over 100000: {{ amountBooksBestsellers }}<br />

				<!-- genre (romance, fantasy, thriller, dark romance) -->
				<!-- tropes (strong female, dominant male lead, bdsm, maffia, occult, college) -->
				<!-- keywords/trigger -->
				<!-- author -->
				<!-- number of pages -->
				<!-- release 2023-11-13 -->

				<!-- reading in app, where to buy -->
			</p>
			<h4>Genre statistics</h4>
			{{ genreStatistics }}

			<h3>Favorite books</h3>
			<p v-if="favoriteBooks.length === 0">No books in favoriteBooks yet</p>
			<ul>
				<li v-for="(book, index) in favoriteBooks" :key="index">
					<h4>{{ book }}</h4>
					<button @click="removeFavoriteBook(book)">remove {{ index }} {{ book }}</button>
				</li>
			</ul>

			<h3>Writers</h3>
			<p v-if="authors.length === 0">No writers defined</p>
			<ul>
				<li v-for="(author, index) in authors" :key="index">{{ author.name }}</li>
			</ul>
			<!-- <h3>Filters</h3> -->
			<!-- <p> -->
			<!-- 	<button @click="showRecent">Only show recent (this decade)</button><br /> -->
			<!-- 	<button @click="showSmallBooks">Only show small books (pages &lt; 200)</button><br /> -->
			<!-- 	<button @click="showSmallChapters"> -->
			<!-- 		Only show books with small chapters (&lt; 10 pages per chapter) -->
			<!-- 	</button> -->
			<!-- </p> -->
			<button @click="incrementCount()">plus</button>

			<h3>Genres</h3>

			<!-- <h4>Romance</h4> -->
			<!-- <ul> -->
			<!-- 	<li v-for="(book,index) of genreRomanceBooks" :key="index">{{book.title}} -->
			<!-- 		({{book.releaseYear}})</li> -->
			<!-- </ul> -->
			<br /><br />
			<!-- <button @click="addTopic('Drama')">add drama</button> -->
			{{ percentIncrease(6244, 12757) }}
		</div>
	</div>
</template>

<script>
export default {
	name: 'BooksOverview',
	data: () => ({
		count: 1,
		showstatsnstuff: false,
		abTitle: null,
		abPages: null,
		abChapters: null,
		abReleaseYear: null,
		abSoldAmount: null,
		abGenres: null,
		abTopics: null,

		booklist: [
			{
				id: 0,
				title: 'Dark Roasted Juan',
				pages: 140,
				chapters: 41,
				releaseYear: 1941,
				soldAmount: 13124,

				author_id: 1,
				genres: ['dark romance', 'thriller', 'drama'],
				topics: ['Spain', 'coffee', 'farmers', 'dominant male lead']
			},
			{
				id: 2,
				title: 'Hunka hunka',
				pages: 200,
				chapters: 31,
				releaseYear: 2001,
				soldAmount: 20010001,

				author_id: 0,
				genres: ['no romance', 'wild'],
				topics: ['strippers', 'workout', 'fitness', 'beach', 'party']
			},
			{
				id: 3,
				title: 'Hookah lounge',
				pages: 20,
				chapters: 3,
				releaseYear: 2022,
				soldAmount: 20,

				author_id: 4,
				genres: ['funk', 'adventure'],
				topics: ['strippers', 'puberty', 'adolecense', 'drugs', 'prositution']
			},
			{
				id: 6,
				title: 'Sweet sweet sweat',
				pages: 170,
				chapters: 30,
				releaseYear: 2021,
				soldAmount: 3214124,

				author_id: 1,
				genres: ['romance', 'thriller'],
				topics: ['candy', 'athletes', 'outdoors', 'dominant female lead', 'running']
			},
			{
				id: 8,
				title: 'Roses and Full moons',
				pages: 800,
				chapters: 8,
				releaseYear: 2008,
				soldAmount: 888888,

				author_id: 4,
				genres: ['romance', 'drama', 'werewolves'],
				topics: ['flowers', 'roses', 'werewolves', 'dominant male lead', 'submissive female lead']
			}
		],
		authors: [
			{
				id: 0,
				name: 'B. Ooba',
				birthdate: '1981-01-30',
				gender: 'F'
			},
			{ id: 1, name: 'L.L. Koel Jee', birthdate: '1969-12-15', gender: 'M' },
			{ id: 4, name: 'Money Penny', birthdate: '1940-05-05', gender: 'F' }
		],
		favoriteBooks: [],
		favoriteTopics: [],
		favoriteGenres: []
	}),
	computed: {
		genreStatistics() {
			return 'stats voor genres'
		},
		amountBigBooks() {
			// console.log('showing big books > 150 pages');
			// console.log('this.booklist:', this.booklist);
			let amountbigbooks = 0
			for (let i = 0; i < this.booklist.length; i++) {
				const element = this.booklist[i]
				if (element.pages > 150) amountbigbooks += 1
			}
			return amountbigbooks
		},
		amountRecentBooks() {
			// console.log('showing recent books, released this decade');
			let amountrecentbooks = 0
			for (let i = 0; i < this.booklist.length; i++) {
				const element = this.booklist[i]
				if (element.releaseYear > 2013) amountrecentbooks += 1
			}
			return amountrecentbooks
		},
		amountBooksSmallChapters() {
			// chapters of <10 pages
			let tmp = 0
			for (let i = 0; i < this.booklist.length; i++) {
				if (this.booklist[i].pages / this.booklist[i].chapters < 10) tmp += 1
			}
			return tmp
		},
		amountBooksBestsellers() {
			let tmp = 0
			for (let i = 0; i < this.booklist.length; i++) {
				if (this.booklist[i].soldAmount > 100000) tmp += 1
			}
			return tmp
		},

		// showRecent() {
		// 	console.log('showing recent')
		// },
		// showSmallBooks() {
		// 	console.log('showing small books')
		// }
		// favoriteBooksArray() {
		// 	return this.favoriteBooks.slice();
		// },
		genreRomanceBooks() {
			return this.booklist.filter((book) => book.genres.indexOf('romance') > -1)
		},
		genreDarkRomanceBooks() {
			return this.booklist.filter((book) => book.genres.indexOf('dark romance') > -1)
		}
	},
	methods: {
		addFavoriteBook(bookName) {
			if (!this.favoriteBooks.includes(bookName)) {
				this.favoriteBooks.push(bookName)
			}
			console.log('favoriteBooks: ', this.favoriteBooks)
		},
		removeFavoriteBook(bookName) {
			this.favoriteBooks.splice(this.favoriteBooks.indexOf(bookName), 1)
			console.log('this.favoriteBooks:', this.favoriteBooks)
		},
		showSmallChapters() {
			console.log('show small chapters')
		},
		incrementCount() {
			this.count += 1
			console.log('this.count:', this.count)
		},
		addTopic(topic) {
			if (!this.favoriteTopics.includes(topic)) this.favoriteTopics.push(topic)
		},
		percentIncrease(oldN, newN) {
			// let diff = newN - oldN
			// let fraction = diff / oldN
			// let percent = fraction * 100
			let smaller = ((newN - oldN) / oldN) * 100
			console.log('percent increase:', smaller)
		},
		addToBooklist(e) {
			console.log('event?', e)
			console.log('this.abTitle:', this.abTitle)
			let newbook = {
				title: this.abTitle,
				releaseYear: this.abReleaseYear,
				pages: this.abPages,
				chapters: this.abChapters,
				genres: this.abGenres.split(','),
				topics: this.abTopics.split(',')
			}
			this.booklist.push(newbook)
			console.log(this.booklist)
		}
	},
	watch: {
		// favoriteBooks(val, oldVal) {
		// 	deep: true;
		// 	console.log('val:', val);
		// 	console.log('oldVal:', oldVal);
		// },
		// favoriteBooksArray(newArray, oldArray) {
		// 	// console.log('newArray: ',newArray);
		// 	// console.log('oldArray: ', oldArray )
		// 	console.log('Favorites updated')
		// },
		count(val, oldVal) {
			console.log(val, oldVal)
		},
		favoriteTopics(val, oldVal) {
			console.log('val', val)
			console.log('oldVal:', oldVal)
		}
	}
}
</script>

<style lang="scss" scoped></style>
