<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import { Github } from 'lucide-vue-next'
</script>

<template>
	<div
		class="min-h-screen bg-hoyo-bg text-gray-100 flex flex-col font-sans selection:bg-blue-500/30 selection:text-blue-200"
	>
		<nav
			class="sticky top-0 z-50 bg-hoyo-bg/80 backdrop-blur-md border-b border-white/5 supports-backdrop-filter:bg-hoyo-bg/60"
		>
			<div
				class="max-w-5xl mx-auto px-4 h-16 flex items-center justify-between"
			>
				<RouterLink to="/" class="flex items-center gap-3 group">
					<div
						class="w-8 h-8 bg-linear-to-br from-blue-500 to-purple-600 rounded-lg flex items-center justify-center font-bold text-white shadow-lg shadow-blue-500/20 group-hover:scale-110 transition-transform duration-300"
					>
						H
					</div>
					<span
						class="font-bold text-lg tracking-tight bg-clip-text text-transparent bg-linear-to-r from-blue-400 to-purple-500"
					>
						Hoyo Code
					</span>
				</RouterLink>

				<div class="flex items-center gap-6 text-sm font-medium">
					<RouterLink
						to="/"
						active-class="text-blue-400"
						class="text-gray-400 hover:text-white transition-colors"
					>
						Home
					</RouterLink>
					<RouterLink
						to="/about"
						active-class="text-blue-400"
						class="text-gray-400 hover:text-white transition-colors"
					>
						About
					</RouterLink>

					<div class="hidden md:block w-px h-4 bg-white/10"></div>

					<a
						href="https://github.com/haiueom/hoyocode-web"
						target="_blank"
						class="text-gray-400 hover:text-white transition-colors"
						title="Source Code"
					>
						<Github class="w-5 h-5" />
					</a>
				</div>
			</div>
		</nav>

		<main class="flex-1 w-full max-w-5xl mx-auto p-4 md:p-8">
			<RouterView v-slot="{ Component }">
				<transition name="fade" mode="out-in">
					<component :is="Component" />
				</transition>
			</RouterView>
		</main>

		<footer class="border-t border-white/5 py-8 text-center">
			<p class="text-sm text-gray-500">
				&copy; 2026 Hoyo Code.
				<span class="block md:inline mt-1 md:mt-0"
					>Not affiliated with HoYoverse. Data provided by Fandom
					Wiki.</span
				>
			</p>
		</footer>
	</div>
</template>

<style>
/* Animasi Transisi Halaman */
.fade-enter-active,
.fade-leave-active {
	transition: opacity 0.2s ease, transform 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
	opacity: 0;
	transform: translateY(5px);
}
</style>
