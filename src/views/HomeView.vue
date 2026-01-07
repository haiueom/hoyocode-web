<script setup lang="ts">
import { ref, watch, onMounted } from 'vue'
import {
	Copy,
	Check,
	Search,
	RotateCcw,
	ChevronLeft,
	ChevronRight,
	ArrowUpDown,
	Filter,
} from 'lucide-vue-next'

// --- KONFIGURASI ---
const API_URL = 'https://hoyocode.haiueom.workers.dev/api/codes'

// --- TIPE DATA ---
interface Reward {
	label: string
	image: string
}
interface DurationItem {
	label: string
	value: string
}
interface GameCode {
	id: number
	game_slug: string
	code: string
	server: string
	rewards: Reward[]
	duration: DurationItem[]
	status: 'Active' | 'Expired'
	redemption_url: string
	last_checked: string
}

// --- STATE ---
const codes = ref<GameCode[]>([])
const loading = ref(false)
const copiedId = ref<number | null>(null)

// State Filter
const selectedGame = ref('all')
const searchTerm = ref('')
const sortOption = ref('date_desc')
const showActiveOnly = ref(true) // State Baru
const currentPage = ref(1)
const totalPages = ref(1)

// --- FETCH DATA ---
const fetchCodes = async () => {
	loading.value = true
	try {
		const [sortBy, order] = sortOption.value.split('_')

		const params = new URLSearchParams({
			page: currentPage.value.toString(),
			limit: '20',
			game: selectedGame.value,
			search: searchTerm.value,
			// FIX: Tambahkan || '' untuk mencegah tipe 'undefined'
			sort_by: sortBy || 'date',
			order: order || 'desc',
			status: showActiveOnly.value ? 'Active' : '',
		})

		const res = await fetch(`${API_URL}?${params.toString()}`)
		const json = await res.json()

		if (json.meta.success) {
			codes.value = json.data
			totalPages.value = json.meta.total_pages
		}
	} catch (error) {
		console.error('Fetch error:', error)
	} finally {
		loading.value = false
	}
}

// --- WATCHERS ---
watch([selectedGame, searchTerm, sortOption, showActiveOnly], () => {
	currentPage.value = 1
	fetchCodes()
})

watch(currentPage, () => {
	fetchCodes()
})

let searchTimeout: number
const handleSearch = (e: Event) => {
	const val = (e.target as HTMLInputElement).value
	clearTimeout(searchTimeout)
	searchTimeout = setTimeout(() => {
		searchTerm.value = val
	}, 500)
}

const copyToClipboard = async (text: string, id: number) => {
	await navigator.clipboard.writeText(text)
	copiedId.value = id
	setTimeout(() => (copiedId.value = null), 2000)
}

const getGameColor = (slug: string) => {
	if (slug === 'genshin')
		return 'text-blue-300 border-blue-500/30 bg-blue-500/10'
	if (slug === 'hsr')
		return 'text-yellow-300 border-yellow-500/30 bg-yellow-500/10'
	if (slug === 'zzz') return 'text-red-300 border-red-500/30 bg-red-500/10'
	return 'text-gray-300'
}

onMounted(() => {
	fetchCodes()
})
</script>

<template>
	<div class="space-y-6">
		<div
			class="sticky top-4 z-10 bg-hoyo-bg/95 backdrop-blur-sm p-2 rounded-xl border border-white/5 shadow-xl flex flex-col md:flex-row md:items-center gap-4"
		>
			<div
				class="flex gap-1 bg-hoyo-card p-1 rounded-lg overflow-x-auto shrink-0"
			>
				<button
					v-for="game in ['all', 'genshin', 'hsr', 'zzz']"
					:key="game"
					@click="selectedGame = game"
					class="px-4 py-2 rounded-md text-sm font-medium transition-all capitalize whitespace-nowrap"
					:class="
						selectedGame === game
							? 'bg-gray-600 text-white shadow-md'
							: 'text-gray-400 hover:text-white'
					"
				>
					{{ game === 'hsr' ? 'Star Rail' : game }}
				</button>
			</div>

			<div class="flex-1 flex flex-col md:flex-row gap-2 w-full">
				<div class="relative flex-1">
					<Search
						class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-500"
					/>
					<input
						:value="searchTerm"
						@input="handleSearch"
						type="text"
						placeholder="Cari kode..."
						class="w-full h-full bg-hoyo-card border border-white/10 rounded-lg pl-9 pr-4 py-2 text-sm focus:outline-none focus:border-blue-500 transition-colors placeholder-gray-600"
					/>
				</div>

				<div class="flex gap-2 shrink-0">
					<label
						class="flex items-center gap-2 px-3 py-2 rounded-lg border cursor-pointer select-none transition-all"
						:class="
							showActiveOnly
								? 'bg-green-500/20 border-green-500/50 text-green-400'
								: 'bg-hoyo-card border-white/10 text-gray-400 hover:bg-white/5'
						"
						title="Tampilkan hanya kode yang aktif"
					>
						<input
							v-model="showActiveOnly"
							type="checkbox"
							class="hidden"
						/>
						<Filter class="w-4 h-4" />
						<span class="text-sm font-medium">Aktif</span>
					</label>

					<div class="relative">
						<div
							class="absolute left-3 top-1/2 -translate-y-1/2 pointer-events-none"
						>
							<ArrowUpDown class="w-4 h-4 text-gray-500" />
						</div>
						<select
							v-model="sortOption"
							class="appearance-none h-full bg-hoyo-card border border-white/10 rounded-lg pl-9 pr-8 py-2 text-sm text-gray-300 focus:outline-none focus:border-blue-500 transition-colors cursor-pointer hover:bg-white/5"
						>
							<option value="date_desc">Terbaru</option>
							<option value="date_asc">Terlama</option>
							<option value="status_asc">Status (Aktif)</option>
							<option value="status_desc">
								Status (Expired)
							</option>
						</select>
					</div>

					<button
						@click="fetchCodes"
						class="p-2 rounded-lg bg-hoyo-card border border-white/10 hover:bg-gray-700 transition flex items-center justify-center shrink-0"
					>
						<RotateCcw
							class="w-5 h-5 text-gray-300"
							:class="{ 'animate-spin': loading }"
						/>
					</button>
				</div>
			</div>
		</div>

		<div v-if="loading && codes.length === 0" class="text-center py-20">
			<div class="animate-bounce mb-4 text-4xl">🎲</div>
			<p class="text-gray-500">Memuat data...</p>
		</div>

		<div
			v-else-if="codes.length === 0"
			class="text-center py-12 text-gray-500 bg-hoyo-card/50 rounded-xl border border-dashed border-white/10"
		>
			<p>Tidak ada kode yang ditemukan.</p>
		</div>

		<div
			v-else
			class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 auto-rows-fr"
		>
			<div
				v-for="item in codes"
				:key="item.id"
				class="bg-hoyo-card rounded-xl border border-white/5 overflow-hidden hover:border-white/20 transition-all group shadow-lg flex flex-col h-full"
				:class="{ 'opacity-60 grayscale': item.status === 'Expired' }"
			>
				<div
					class="p-4 flex justify-between items-start border-b border-white/5 bg-black/20"
				>
					<span
						class="text-xs font-bold px-2 py-1 rounded border capitalize"
						:class="getGameColor(item.game_slug)"
						>{{ item.game_slug }}</span
					>
					<span
						class="text-xs px-2 py-1 rounded font-medium"
						:class="
							item.status === 'Active'
								? 'bg-green-500/20 text-green-400 border border-green-500/30'
								: 'bg-red-500/20 text-red-400 border border-red-500/30'
						"
						>{{ item.status }}</span
					>
				</div>
				<div class="p-4 flex-1 flex flex-col gap-4">
					<div
						class="bg-black/40 p-3 rounded-lg flex justify-between items-center group-hover:bg-black/60 transition border border-white/5"
					>
						<code
							class="text-lg font-mono font-bold tracking-wide text-white select-all truncate"
							>{{ item.code }}</code
						>
						<button
							@click="copyToClipboard(item.code, item.id)"
							class="p-1.5 rounded-md hover:bg-white/10 transition text-gray-400 hover:text-white shrink-0 ml-2"
						>
							<Check
								v-if="copiedId === item.id"
								class="w-4 h-4 text-green-400"
							/><Copy v-else class="w-4 h-4" />
						</button>
					</div>
					<div>
						<div class="flex flex-wrap gap-2">
							<div
								v-for="(reward, idx) in item.rewards.slice(
									0,
									4
								)"
								:key="idx"
								class="flex items-center gap-2 bg-white/5 px-2 py-1.5 rounded-md text-xs border border-white/5"
							>
								<img
									v-if="reward.image"
									:src="reward.image"
									class="w-5 h-5 object-contain"
									loading="lazy"
								/>
								<span
									v-else
									class="text-gray-300 font-medium"
									>{{ reward.label }}</span
								>
								<span
									v-if="reward.image"
									class="text-gray-300 font-medium truncate max-w-37.5"
									>{{ reward.label }}</span
								>
							</div>
							<div
								v-if="item.rewards.length > 4"
								class="flex items-center justify-center bg-white/10 px-3 py-1.5 rounded-md text-xs border border-white/10 text-gray-400"
							>
								+ {{ item.rewards.length - 4 }}
							</div>
						</div>
					</div>
					<div
						class="mt-auto pt-3 border-t border-white/5 grid grid-cols-1 gap-1"
					>
						<div
							v-for="(dur, i) in item.duration"
							:key="i"
							class="text-xs text-gray-500 flex justify-between items-start"
						>
							<span
								class="font-medium text-gray-600 shrink-0 mr-2"
								>{{ dur.label }}:</span
							>
							<span
								class="text-gray-400 text-right truncate max-w-[70%]"
								:title="dur.value"
								>{{ dur.value }}</span
							>
						</div>
					</div>
				</div>
				<div class="p-3 bg-black/30 flex gap-2 border-t border-white/5">
					<a
						v-if="item.redemption_url && item.status === 'Active'"
						:href="item.redemption_url"
						target="_blank"
						class="flex-1 flex items-center justify-center gap-2 bg-blue-600 hover:bg-blue-500 text-white py-2 rounded-lg text-sm font-bold transition"
						>Tukar Sekarang</a
					>
					<button
						v-else
						disabled
						class="flex-1 bg-white/5 text-gray-500 py-2 rounded-lg text-sm font-medium cursor-not-allowed border border-white/5"
					>
						Link Tidak Tersedia
					</button>
				</div>
			</div>
		</div>

		<div
			v-if="totalPages > 1"
			class="flex justify-center items-center gap-4 pt-6 pb-8"
		>
			<button
				@click="currentPage--"
				:disabled="currentPage === 1"
				class="p-2 rounded-lg bg-hoyo-card border border-white/10 hover:bg-white/10 disabled:opacity-50 disabled:cursor-not-allowed transition text-gray-300"
			>
				<ChevronLeft class="w-5 h-5" />
			</button>
			<span class="text-sm text-gray-400 font-medium"
				>Page <span class="text-white">{{ currentPage }}</span> /
				<span class="text-white">{{ totalPages }}</span></span
			>
			<button
				@click="currentPage++"
				:disabled="currentPage === totalPages"
				class="p-2 rounded-lg bg-hoyo-card border border-white/10 hover:bg-white/10 disabled:opacity-50 disabled:cursor-not-allowed transition text-gray-300"
			>
				<ChevronRight class="w-5 h-5" />
			</button>
		</div>
	</div>
</template>
