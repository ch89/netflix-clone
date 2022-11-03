<script setup>
	import Navbar from "./components/Navbar.vue"
	import Banner from "./components/Banner.vue"
	import Slide from "./components/Slide.vue"
	// import Slide from "./components/Slide2.vue"

	const slides = [
		{ id: 28, title: "Action" },
		{ id: 27, title: "Horror" },
		{ id: 10749, title: "Romance" },
	]
</script>

<template>
	<navbar></navbar>
	<banner></banner>
	<!-- <main class="p-16 grid gap-16">
		<slide v-for="slide in slides" :key="slide.id" v-bind="slide"></slide>
	</main> -->
	<!-- <main class="py-16 grid gap-16">
		<slide v-for="slide in slides" :key="slide.id" v-bind="slide"></slide>
	</main> -->

	<main class="p-14 grid gap-16">
		<slide v-for="slide in slides" :key="slide.id" v-bind="slide"></slide>
	</main>
</template>

<style>
	.next-enter-from, .prev-leave-to {
		@apply translate-x-full;
	}

	.next-leave-to, .prev-enter-from {
		@apply -translate-x-full;
	}
</style>