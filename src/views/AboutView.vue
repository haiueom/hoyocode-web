<script setup lang="ts">
import {
	Code2,
	Database,
	Layout,
	Zap,
	ShieldCheck,
	Clock,
	Github,
	Heart,
	Server,
} from 'lucide-vue-next'
</script>

<template>
	<div class="space-y-8 animate-fade-in">
		<div class="text-center space-y-4 py-8">
			<div
				class="inline-flex items-center justify-center p-3 rounded-2xl bg-linear-to-br from-blue-500/20 to-purple-600/20 border border-white/10 mb-4 shadow-xl shadow-blue-900/10"
			>
				<div
					class="w-12 h-12 bg-linear-to-br from-blue-500 to-purple-600 rounded-xl flex items-center justify-center font-bold text-2xl text-white shadow-inner"
				>
					H
				</div>
			</div>
			<h1
				class="text-4xl md:text-5xl font-bold bg-clip-text text-transparent bg-linear-to-r from-blue-400 via-purple-400 to-blue-400 bg-size-[200%_auto] animate-gradient"
			>
				Hoyo Code
			</h1>
			<p class="text-gray-400 max-w-2xl mx-auto text-lg leading-relaxed">
				Agregator kode redeem otomatis untuk Genshin Impact, Honkai:
				Star Rail, dan Zenless Zone Zero. Data diperbarui secara
				real-time dari Wiki Fandom tanpa intervensi manual.
			</p>
		</div>

		<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
			<div
				class="bg-hoyo-card p-6 rounded-2xl border border-white/5 hover:border-blue-500/30 transition-all group"
			>
				<div class="flex items-center gap-3 mb-6">
					<div
						class="p-2 bg-blue-500/10 rounded-lg text-blue-400 group-hover:bg-blue-500 group-hover:text-white transition-colors"
					>
						<Server class="w-6 h-6" />
					</div>
					<h2 class="text-xl font-bold text-white">
						Backend Infrastructure
					</h2>
				</div>

				<ul class="space-y-4">
					<li class="flex items-start gap-3">
						<Zap class="w-5 h-5 text-yellow-400 shrink-0 mt-0.5" />
						<div>
							<span class="block text-white font-medium"
								>Cloudflare Workers</span
							>
							<p class="text-sm text-gray-500">
								Serverless compute yang berjalan di tepi
								jaringan (Edge) untuk performa ultra-cepat.
							</p>
						</div>
					</li>
					<li class="flex items-start gap-3">
						<Database
							class="w-5 h-5 text-green-400 shrink-0 mt-0.5"
						/>
						<div>
							<span class="block text-white font-medium"
								>Cloudflare D1 (SQLite)</span
							>
							<p class="text-sm text-gray-500">
								Database SQL terdistribusi untuk menyimpan
								riwayat kode dan status.
							</p>
						</div>
					</li>
					<li class="flex items-start gap-3">
						<Clock
							class="w-5 h-5 text-purple-400 shrink-0 mt-0.5"
						/>
						<div>
							<span class="block text-white font-medium"
								>Cron Triggers</span
							>
							<p class="text-sm text-gray-500">
								Penjadwalan otomatis setiap 1 jam untuk
								melakukan scraping data terbaru.
							</p>
						</div>
					</li>
					<li class="flex items-start gap-3">
						<Code2 class="w-5 h-5 text-pink-400 shrink-0 mt-0.5" />
						<div>
							<span class="block text-white font-medium"
								>Hono + Cheerio</span
							>
							<p class="text-sm text-gray-500">
								Framework API ultra-ringan dan library parsing
								HTML yang efisien.
							</p>
						</div>
					</li>
				</ul>
			</div>

			<div
				class="bg-hoyo-card p-6 rounded-2xl border border-white/5 hover:border-purple-500/30 transition-all group"
			>
				<div class="flex items-center gap-3 mb-6">
					<div
						class="p-2 bg-purple-500/10 rounded-lg text-purple-400 group-hover:bg-purple-500 group-hover:text-white transition-colors"
					>
						<Layout class="w-6 h-6" />
					</div>
					<h2 class="text-xl font-bold text-white">
						Frontend Experience
					</h2>
				</div>

				<ul class="space-y-4">
					<li class="flex items-start gap-3">
						<Zap class="w-5 h-5 text-green-400 shrink-0 mt-0.5" />
						<div>
							<span class="block text-white font-medium"
								>Vue 3 + Vite</span
							>
							<p class="text-sm text-gray-500">
								Framework progresif dengan build tool generasi
								terbaru untuk performa instan.
							</p>
						</div>
					</li>
					<li class="flex items-start gap-3">
						<Layout class="w-5 h-5 text-blue-400 shrink-0 mt-0.5" />
						<div>
							<span class="block text-white font-medium"
								>Tailwind CSS</span
							>
							<p class="text-sm text-gray-500">
								Utility-first CSS framework untuk desain
								antarmuka yang modern dan responsif.
							</p>
						</div>
					</li>
					<li class="flex items-start gap-3">
						<ShieldCheck
							class="w-5 h-5 text-orange-400 shrink-0 mt-0.5"
						/>
						<div>
							<span class="block text-white font-medium"
								>TypeScript</span
							>
							<p class="text-sm text-gray-500">
								Keamanan tipe data statis untuk mencegah bug
								saat pengembangan.
							</p>
						</div>
					</li>
					<li class="flex items-start gap-3">
						<Github class="w-5 h-5 text-white shrink-0 mt-0.5" />
						<div>
							<span class="block text-white font-medium"
								>Cloudflare Pages</span
							>
							<p class="text-sm text-gray-500">
								Hosting statis yang terintegrasi langsung dengan
								jaringan global Cloudflare.
							</p>
						</div>
					</li>
				</ul>
			</div>
		</div>

		<div
			class="bg-black/20 p-6 rounded-2xl border border-white/5 text-center space-y-4"
		>
			<p class="text-sm text-gray-500 max-w-3xl mx-auto">
				<strong>Disclaimer:</strong> Aplikasi ini adalah proyek
				komunitas (Fan-made). HoyoCodes tidak berafiliasi, didukung,
				disponsori, atau disetujui secara khusus oleh
				<span class="text-gray-300"
					>Cognosphere Pte. Ltd. (HoYoverse)</span
				>. Semua gambar, aset game, dan nama dagang adalah hak cipta
				pemiliknya masing-masing.
			</p>

			<div
				class="flex items-center justify-center gap-2 text-sm text-gray-400 pt-4 border-t border-white/5"
			>
				<span>Dibuat dengan</span>
				<Heart
					class="w-4 h-4 text-red-500 fill-red-500 animate-pulse"
				/>
				<span>oleh Haiueom</span>
				<span class="mx-2">•</span>
				<a
					href="https://github.com/haiueom/hoyocode-web"
					target="_blank"
					class="flex items-center gap-1 hover:text-white transition"
				>
					<Github class="w-4 h-4" />
					Source Code
				</a>
			</div>
		</div>
	</div>
</template>

<style scoped>
/* Animasi Gradasi Teks */
@keyframes gradient {
	0% {
		background-position: 0% 50%;
	}
	50% {
		background-position: 100% 50%;
	}
	100% {
		background-position: 0% 50%;
	}
}

.animate-gradient {
	animation: gradient 6s ease infinite;
}

/* Animasi Fade In saat halaman dimuat */
.animate-fade-in {
	animation: fadeIn 0.5s ease-out;
}

@keyframes fadeIn {
	from {
		opacity: 0;
		transform: translateY(10px);
	}
	to {
		opacity: 1;
		transform: translateY(0);
	}
}
</style>
