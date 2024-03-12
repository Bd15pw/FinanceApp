<script setup>
import { transactionViewOptions } from "~/constans";

const viewselected = ref(transactionViewOptions[0]);
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
		<Transaction />
		<Transaction />
		<Transaction />
		<Transaction />
	</section>
</template>
