<script setup>
const props = defineProps({
	transaction: Object,
});

const isIncome = computed(() => props.transaction.type === "Income");

// ikona na wydatki lub przychody

const icon = computed(() =>
	isIncome.value
		? "i-heroicons-arrow-trending-up"
		: "i-heroicons-arrow-trending-down"
);

const iconColor = computed(() =>
	isIncome.value ? "text-green-600" : "text-red-600"
);

const { currency } = useCurrency(props.transaction.amount);

const supabase = useSupabaseClient();
const isLoading = ref(false);
const toast = useToast();

const deleteTransaction = async () => {
	isLoading.value = true;

	try {
		await supabase.from("transaction").delete().eq("id", props.transaction.id);
		toast.add({
			title: "Transaction deleted",
			icon: "i-heroicons-check-circle",
			color: "green",
		});
	} catch (error) {
		toast.add({
			title: "Transaction is not deleted",
			icon: "i-heroicons-exclamation-circle",
			color: "red",
		});
	} finally {
		isLoading.value = false;
	}
};

const items = [
	[
		{
			label: "Edit",
			icon: "i-heroicons-pencil-square-20-solid",
			click: () => console.log("Edit"),
		},
		{
			label: "Delete",
			icon: "i-heroicons-trash-20-solid",
			click: deleteTransaction,
		},
	],
];
</script>
<template>
	<div
		class="grid grid-cols-2 py-2 border-b border-gray-200 dark:border-gray-800 text-gray-900 dark:text-gray-100"
	>
		<div class="flex items-center justify-between">
			<div class="flex items-center justify-between space-x-2">
				<UIcon :name="icon" :class="iconColor" />
				<div>{{ transaction.description }}</div>
			</div>
			<div>
				<UBadge color="cyan" variant="subtle" v-if="transaction.category">
					{{ transaction.category }}</UBadge
				>
			</div>
		</div>
		<div class="flex items-center justify-end space-x-2">
			<div>{{ currency }}</div>
			<div>
				<UDropdown :items="items" :popper="{ placement: 'bottom-start' }">
					<UButton
						color="cyan"
						variant="ghost"
						trailing-icon="i-heroicons-ellipsis-horizontal"
						:loading="isLoading"
					/>
				</UDropdown>
			</div>
		</div>
	</div>
</template>
