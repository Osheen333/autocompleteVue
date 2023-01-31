<template>
	<div id="app">
		<SearchAutocomplete
			@blur="handleSearch"
			@input="handleSearch"
			v-model="search"
			:items="searchResult"
		/>
	</div>
</template>

<script>
import SearchAutocomplete from '../components/SearchAutocomplete.vue'
import axios from 'axios';

export default {
	name: 'App',
	components: {
		SearchAutocomplete
	},
	data() {
		return {
			searchResult: [],
			search: ''
		};
	},
	methods: {
		handleSearch() {
			setTimeout(() => {
				const res = axios
					.get(`https://us1.locationiq.com/v1/search?key=pk.e87810ce3ac416dd6ed9e4195717bd04`, { params: { q: this.search, format: 'json' } })
					.then(response => (this.searchResult = response.data.map(a => a.display_name)))
			}, 1000);


		}
	},
	mounted() {
		if (this.search) {
			this.handleSearch()
		}
	},
}
</script>