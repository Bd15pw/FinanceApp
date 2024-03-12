<script setup>
const { currency } = useCurrency(3000);
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
			click: () => console.log("Delete"),
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
				<UIcon
					name="i-heroicons-arrow-trending-up-16-solid"
					class="text-green-600"
				/>
				<div>test</div>
			</div>
			<div>
				<UBadge color="cyan" variant="subtle"> Category</UBadge>
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
					/>
				</UDropdown>
			</div>
		</div>
	</div>
</template>
