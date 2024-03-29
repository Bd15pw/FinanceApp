<script setup>
import { transactionViewOptions } from "~/constans";

const viewselected = ref(transactionViewOptions[0]);

const supabase = useSupabaseClient();

const transactions = ref([]);
const isLoading = ref(false);

// pobieranie tranzakcji z supabase
const fetchTransactions = async () => {
	isLoading.value = true;
	try {
		const { data } = await useAsyncData("transaction", async () => {
			const { data, error } = await supabase.from("transaction").select();

			if (error) return [];
			return data;
		});

		return data.value;
	} finally {
		isLoading.value = false;
	}
};

const refreshTransaction = async () =>
	(transactions.value = await fetchTransactions());

await refreshTransaction();

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

// Zliczanie wydatków i przychodów

const income = computed(() =>
	transactions.value.filter((t) => t.type === "Income")
);

const expense = computed(() =>
	transactions.value.filter((t) => t.type === "Expense")
);

const incomeCount = computed(() => income.value.length);
const expenseCount = computed(() => expense.value.length);

const incomeTotal = computed(() =>
	income.value.reduce((sum, transaction) => sum + transaction.amount, 0)
);

const expenseTotal = computed(() =>
	expense.value.reduce((sum, transaction) => sum + transaction.amount, 0)
);
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
			:amount="incomeTotal"
			:last-amount="2000"
			:loading="isLoading"
		/>
		<Trend
			color="red"
			title="Expense"
			:amount="expenseTotal"
			:last-amount="44000"
			:loading="isLoading"
		/>
		<Trend
			color="red"
			title="Investments"
			:amount="4000"
			:last-amount="33000"
			:loading="isLoading"
		/>
		<Trend
			color="red"
			title="Savings"
			:amount="4000"
			:last-amount="3000"
			:loading="isLoading"
		/>
	</section>

	<section class="flex justify-between mb-10">
		<div>
			<h2 class="text-2xl font-extrabold">Transactions</h2>
			<div class="text-gray-500 dark:text-gray-400">
				You have {{ incomeCount }} incomes and {{ expenseCount }} expenses this
				period
			</div>
		</div>
		<div>
			<UButton
				icon="i-heroicons-plus-circle"
				color="white"
				variant="solid"
				label="Add"
			/>
		</div>
	</section>

	<section v-if="!isLoading">
		<div
			v-for="(transactionsOnDay, date) in transactionGroupedByDate"
			:key="date"
			class="mb-10"
		>
			<DailyTransaction :date="date" :transactions="transactionsOnDay" />
			<Transaction
				v-for="transaction in transactionsOnDay"
				:key="transaction.id"
				:transaction="transaction"
				@deleted="refreshTransaction()"
			/>
		</div>
	</section>
	<section v-else>
		<USkeleton class="h-8 w-full xmb-2" v-for="i in 4" :key="i" />
	</section>
</template>
