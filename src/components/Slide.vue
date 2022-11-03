<script setup>
	import { ref } from "vue"

	const props = defineProps(["id", "title"])
	const movies = ref([])
	const slides = ref([])
	const index = ref(0)
	const disabled = ref(false)
	const name = ref("")

	fetch(`https://api.themoviedb.org/3/discover/movie?api_key=664b88830136f3ffbf5fd056348144bc&with_genres=${props.id}`)
		.then(response => response.json())
		// .then(data => movies.value = data.results.slice(0, 18))
		.then(data => {
			for(let i = 0; i < data.results.slice(0, 18).length; i += 6) {
				slides.value.push(data.results.slice(i, i + 6))
			}
		})
</script>

<template>
	<!-- <div class="p-16 flex gap-4 overflow-auto scrollbar-hide snap-x snap-mandatory">
		<img :src="`https://image.tmdb.org/t/p/original${backdrop_path}`" v-for="{ backdrop_path } in movies" class="w-[calc(20%-.8rem)] rounded-xl snap-center hover:scale-105 transition duration-500">
	</div> -->

	<!-- <div>
		<h3 class="text-white text-xl font-bold ml-16">Action</h3>
		<div class="px-16 py-2 flex gap-4 overflow-auto scrollbar-hide snap-x snap-mandatory">
			<img :src="`https://image.tmdb.org/t/p/original${backdrop_path}`" v-for="{ backdrop_path } in movies" class="w-[calc(20%-.8rem)] rounded-xl snap-center hover:scale-105 transition duration-500">
		</div>
	</div> -->

	<div class="text-white grid grid-areas gap-x-2 text-xl">
		<h3 class="font-bold ml-2">{{ title }}</h3>
		<button @click="index ? index-- : index = slides.length - 1; name = 'prev'" :disabled="disabled">
			<i class="fa-solid fa-angle-left"></i>
		</button>
		<div class="relative overflow-hidden">
			<transition v-for="(slide, i) in slides" :name="name" leave-active-class="absolute top-0" @enter="disabled = true" @after-enter="disabled = false">
				<div class="grid grid-flow-col auto-cols-fr gap-4 p-2 transition duration-1000" v-show="i == index">
					<img :src="`https://image.tmdb.org/t/p/original${backdrop_path}`" v-for="{ backdrop_path } in slide" class="rounded-xl hover:scale-105 transition duration-500">
				</div>
			</transition>
		</div>
		<button @click="index == slides.length - 1 ? index = 0 : index++; name = 'next'" :disabled="disabled">
			<i class="fa-solid fa-angle-right"></i>
		</button>
	</div>
</template>

<style scoped>
	.grid-areas {
		grid-template-areas:
			". title ."
			"bl slider br"
	}

	.grid-areas > :nth-child(1) {
		grid-area: title;
	}

	.grid-areas > :nth-child(2) {
		grid-area: bl;
	}

	.grid-areas > :nth-child(3) {
		grid-area: slider;
	}

	.grid-areas > :nth-child(4) {
		grid-area: br;
	}
</style>