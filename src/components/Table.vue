<template>
  <div class="overflow-x-auto">
    <table class="min-w-full divide-y divide-gray-300">
      <thead>
        <tr>
          <th
            scope="col"
            class="py-3.5 pl-4 pr-3 text-left text-sm font-semibold text-gray-900 sm:pl-0"
          >
            Description
          </th>
          <th
            scope="col"
            class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900"
          >
            Amount
          </th>
          <th scope="col" class="relative py-3.5 pl-3 pr-4 sm:pr-0">
            <span class="sr-only">Edit</span>
          </th>
        </tr>
      </thead>
      <tbody class="divide-y divide-gray-200">
        <tr v-for="(Transaction, index) in TransactionProp" :key="index">
          <td
            class="whitespace-nowrap py-2 sm:py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-0"
          >
            {{ Transaction.description }}
          </td>
          <td class="whitespace-nowrap px-3 py-2 sm:py-4 text-sm text-gray-500">
            {{ Transaction.amount }}
          </td>
          <td
            class="relative whitespace-nowrap py-2 sm:py-4 pl-3 pr-4 text-right text-sm font-medium sm:pr-0"
          >
            <a
              @click="$emit('deleteTransaction', index)"
              class="text-indigo-600 hover:text-indigo-900"
              >Delete<span class="sr-only"></span
            ></a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup lang="ts">
defineProps(["TransactionProp"]);
defineEmits(["deleteTransaction"]);
</script>
