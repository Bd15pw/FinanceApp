<script setup>
const props = defineProps({
	title: String,
	amount: Number,
	lastAmount: Number,
	color: String,
	loading: Boolean,
});

const trendingUp = computed(() => props.amount >= props.lastAmount);

const icon = computed(() =>
	trendingUp.value
		? "i-heroicons-arrow-trending-up-16-solid"
		: "i-heroicons-arrow-trending-down-16-solid"
);

const { currency } = useCurrency(props.amount);

const precentageTrend = computed(() => {
	if (props.amount === 0 || props.lastAmount === 0) return "∞%";

	const bigger = Math.max(props.amount, props.lastAmount);
	const lower = Math.min(props.amount, props.lastAmount);

	const ratio = ((bigger - lower) / lower) * 100;

	return `${Math.ceil(ratio)}%`;
});
</script>
<template>
	<div>
		<div class="font-bold" :class="[color]">{{ title }}</div>
		<div class="text-2xl font-extrabold text-black dark:text-white mb-2">
			<USkeleton class="h-8 w-full" v-if="loading" />
			<div v-else>{{ currency }}</div>
		</div>
		<div>
			<USkeleton class="h-6 w-full" v-if="loading" />
			<div v-else class="flex space-x-1 items-center text-sm">
				<UIcon
					:name="icon"
					class="w-6 h-6"
					:class="{ green: trendingUp, red: !trendingUp }"
				/>
				<div class="text-gray-500 dark:text-gray-400">
					{{ precentageTrend }} vs last period
				</div>
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
