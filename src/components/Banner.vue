<script setup>
	import { ref, computed, watch } from "vue"

	const movies = ref([])
	const index = ref(0)
	const disabled = ref(false)
	const name = ref("")
	const duration = 7000

	const movie = computed(() => movies.value[index.value])

	let prev = e => {
		index.value ? index.value-- : index.value = movies.value.length - 1
		name.value = 'prev'
	}

	let next = e => {
		index.value == movies.value.length - 1 ? index.value = 0 : index.value++
		name.value = 'next'
	}

	let timer = setTimeout(next, duration)

	watch(index, () => {
		clearTimeout(timer)
		timer = setTimeout(next, duration)
	})

	document.addEventListener("visibilitychange", e => {
		e.target.hidden ? clearTimeout(timer) : timer = setTimeout(next, duration)
	})

	fetch("https://api.themoviedb.org/3/trending/all/week?api_key=664b88830136f3ffbf5fd056348144bc")
		.then(response => response.json())
		.then(data => movies.value = data.results.slice(0, 5))
</script>

<template>
	<div v-if="movies.length" class="relative overflow-hidden">
		<transition enter-from-class="opacity-0" leave-to-class="opacity-0" leave-active-class="absolute" @enter="disabled = true" @after-enter="disabled = false">
		<!-- <transition :name="name" leave-active-class="absolute" @enter="disabled = true" @after-enter="disabled = false"> -->
			<div class="transition duration-1000" :key="index">
				<img :src="`https://image.tmdb.org/t/p/original${movie.backdrop_path}`" class="h-[32rem] w-screen object-cover">
				<div class="bg-gradient-to-t from-[#141414] h-[7rem] absolute w-full bottom-0"></div>
				<div class="absolute bottom-16 left-16 text-white w-[42rem] space-y-6">
					<h2 class="text-5xl font-bold drop-shadow-[2px_2px_4px_rgb(0,0,0,.45)]">{{ movie.title || movie.name }}</h2>
					<p class="text-xl drop-shadow-[2px_2px_4px_rgb(0,0,0,.45)] line-clamp-3">{{ movie.overview }}</p>
					<button class="bg-white px-6 py-3 rounded-lg font-bold text-[#141414] mr-4">
						<i class="fa-solid fa-play"></i> Play
					</button>
					<button class="bg-[rgba(109,109,110,0.7)] px-6 py-3 rounded-lg font-bold">
						<i class="fa-solid fa-circle-info"></i> More Info
					</button>
				</div>
			</div>
		</transition>
		<button @click="prev" class="text-white text-4xl absolute left-4 top-1/2 -translate-y-1/2" :disabled="disabled">
			<i class="fa-solid fa-angle-left"></i>
		</button>
		<button @click="next" class="text-white text-4xl absolute right-4 top-1/2 -translate-y-1/2" :disabled="disabled">
			<i class="fa-solid fa-angle-right"></i>
		</button>
		<div class="absolute bottom-4 inset-x-0 flex justify-center gap-4">
			<button v-for="(movie, i) in movies" class="w-4 h-4 rounded-full border-2 border-white" :class="{ 'bg-white': i == index }" @click="name = i > index ? 'next' : 'prev'; index = i" :disabled="disabled"></button>
		</div>
	</div>
</template>