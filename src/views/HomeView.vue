<template>
	<div id="app">
		<SearchAutocomplete :items="searchResult" @search="handleSearch" />
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
		};
	},
	methods: {
		 handleSearch(data) {
			const res =  axios
				.get(`https://us1.locationiq.com/v1/search?key=pk.e87810ce3ac416dd6ed9e4195717bd04`, { params: { q: data, format: 'json' } })
				.then(response => (this.searchResult = response.data.map(a => a.display_name)))
			

		}
	},
}
</script>