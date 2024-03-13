<script setup>
import { transactionViewOptions } from "~/constans";

const viewselected = ref(transactionViewOptions[0]);

const supabase = useSupabaseClient();

const transactions = ref([]);

const { data, pending } = await useAsyncData("transaction", async () => {
	const { data, error } = await supabase.from("transaction").select();

	if (error) return [];
	return data;
});
transactions.value = data.value;

//grupowanie tranzakcji po dacie

const transactionGroupedByDate = computed(() => {
	let grouped = {};

	for (const transaction of transactions.value) {
		const date = new Date(transaction.created_at).toISOString().split("T")[0];

		if (!grouped[date]) {
			grouped[date] = [];
		}

		grouped[date].push(transaction);
	}

	return grouped;
});
</script>

<template>
	<section class="flex items-center justify-between mb-10">
		<h1 class="text-4xl font-extrabold">Summary</h1>
		<div>
			<USelectMenu v-model="viewselected" :options="transactionViewOptions" />
		</div>
	</section>
	<section
		class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 sm:gap-16 mb-10"
	>
		<Trend
			color="green"
			title="Income"
			:amount="4000"
			:last-amount="2000"
			:loading="false"
		/>
		<Trend
			color="red"
			title="Expense"
			:amount="4000"
			:last-amount="44000"
			:loading="false"
		/>
		<Trend
			color="red"
			title="Investments"
			:amount="4000"
			:last-amount="33000"
			:loading="false"
		/>
		<Trend
			color="red"
			title="Savings"
			:amount="4000"
			:last-amount="3000"
			:loading="false"
		/>
	</section>

	<section>
		<Transaction
			v-for="transaction in transactions"
			:key="transaction.id"
			:transaction="transaction"
		/>
	</section>
</template>
