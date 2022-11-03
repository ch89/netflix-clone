<script setup>
	import { ref } from "vue"

	const props = defineProps(["id", "title"])
	const movies = ref([])

	fetch(`https://api.themoviedb.org/3/discover/movie?api_key=664b88830136f3ffbf5fd056348144bc&with_genres=${props.id}`)
		.then(response => response.json())
		.then(data => movies.value = data.results.map(movie => movie.backdrop_path))
</script>

<template>
	<div>
		<h3 class="text-white text-xl font-bold ml-2">{{ title }}</h3>
		<!-- px-14 -->
		<div class="flex overflow-auto scrollbar-hide snap-x snap-mandatory">
			<div class="w-[14.285%] shrink-0 p-2" v-for="movie in movies">
				<img :src="`https://image.tmdb.org/t/p/original${movie}`" class="rounded-xl snap-center hover:scale-105 transition duration-500">
			</div>
		</div>
	</div>
</template>