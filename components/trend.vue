<script setup>
defineProps({
	title: String,
	amount: Number,
	lastAmount: Number,
	color: String,
	loading: Boolean,
});
</script>
<template>
	<div>
		<div class="font-bold" :class="[color]">{{ title }}</div>
		<div class="text-2xl font-extrabold text-black dark:text-white mb-2">
			<USkeleton class="h-8 w-full" v-if="loading" />
			<div v-else>{{ amount }}</div>
		</div>
		<div>
			<USkeleton class="h-6 w-full" v-if="loading" />
			<div v-else class="flex space-x-1 items-center text-sm">
				<UIcon
					name="i-heroicons-arrow-trending-up-16-solid"
					class="w-6 h-6"
					:class="[color]"
				/>
				<div class="text-gray-500 dark:text-gray-400">30% vs last period</div>
			</div>
		</div>
	</div>
</template>

<style scoped>
.green {
	@apply text-green-600;
}

.red {
	@apply text-red-600;
}
</style>
