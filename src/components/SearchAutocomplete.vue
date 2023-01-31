<template>
	<div class="autocomplete">
		<input
			type="text"
			v-model="search"
			placeholder="Type address"
			@input="onChange"
			@keydown.down="onArrowDown"
			@keydown.up="onArrowUp"
			@keydown.enter="onEnter"
		/>
		<ul v-if="isOpen" class="dropdown-content">
			<template v-if="searchResult.length">
				<li
					v-for="(result, i) in searchResult"
					:key="i"
					class="autocomplete-result"
					@click="setResult(result)"
					:class="{ 'is-active': i === arrowCounter }"
				>{{ result }}</li>
			</template>
			<div v-if="search &&  !searchResult.length">no result found</div>
		</ul>
	</div>
</template>

<script>
export default {
	name: 'SearchAutocomplete',
	props: {
		items: {
			type: Array,
			required: false,
			default: () => [],
		},
	},
	data() {
		return {
			search: '',
			results: [],
			isOpen: false,
			arrowCounter: -1

		};
	},
	computed: {
		searchResult() {
			return this.search ? this.items : []
		}
	},
	mounted() {
		document.addEventListener('click', this.handleClickOutside);

	},
	destroyed() {
		document.removeEventListener('click', this.handleClickOutside);
	},
	methods: {
		setResult(result) {
			this.search = result;
			this.isOpen = false;
		},

		onChange() {
			setTimeout(() => {

				this.$emit('search', this.search);
			}, 1000);
			this.isOpen = true;

			// this.filterResults();

		},
		handleClickOutside(event) {
			if (!this.$el.contains(event.target)) {
				this.arrowCounter = -1;

				this.isOpen = false;
			}
		},
		onArrowDown() {
			if (this.arrowCounter < this.searchResult.length) {
				this.arrowCounter = this.arrowCounter + 1;
			}
		},
		onArrowUp() {
			if (this.arrowCounter > 0) {
				this.arrowCounter = this.arrowCounter - 1;
			}
		},
		onEnter() {
			this.search = this.searchResult[this.arrowCounter];
			this.arrowCounter = -1;
			this.isOpen = false;
		},

	},
}
</script>

<style>
input {
	box-sizing: border-box;
	height: 35px;
	width: 350px;
	padding: 1rem 1.5rem;
	outline: none;
	border: none;
	border-radius: 25px;
	box-shadow: 0 2px 2px rgba(15, 15, 51, 0.4);
	font-size: 1rem;
	color: rgba(15, 15, 51, 0.8);
	background-color: #e5e5e5;
}
.autocomplete {
	position: relative;
}

.autocomplete-results {
	padding: 0;
	margin: 0;
	border: 1px solid #eeeeee;
	height: 120px;
	min-height: 1em;
	max-height: 6em;
	overflow: auto;
}

.autocomplete-result {
	list-style: none;
	text-align: left;
	padding: 4px 2px;
	cursor: pointer;
	border-bottom: 1px solid #dfdfdf;
}
.autocomplete-result.is-active,
.autocomplete-result:hover {
	background-color: #4aae9b;
	color: white;
}
.dropdown-content {
	position: absolute;
	background-color: #f6f6f6;
	min-width: 350px;
	overflow: auto;
	border: 1px solid #ddd;
	z-index: 1;
	padding: 0.6rem !important;
	border-radius: 0.5rem;
	top: 38px;
}

.dropdown-content a {
	color: black;
	padding: 12px 16px;
	text-decoration: none;
	display: block;
}

.dropdown-content li:hover {
	background-color: #ddd;
}
</style>