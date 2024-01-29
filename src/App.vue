<script setup>
import Table from "./components/Table.vue";
import { ref, computed } from "vue";

const transactionData = ref({
  description: "",
  amount: "",
});

const Transaction = ref([]);

const addTransaction = () => {
  Transaction.value.push({ ...transactionData.value });
  transactionData.value = {
    description: "",
    amount: "",
  };
};

const deleteTransaction = (index) => {
  Transaction.value.splice(index, 1);
};

const income = computed(() => {
  let sum = 0;
  Transaction.value.forEach((transaction) => {
    if (transaction.amount >= 0) {
      sum = sum + transaction.amount;
    }
  });
  return sum;
});

const expenses = computed(() => {
  let sum = 0;
  Transaction.value.forEach((transaction) => {
    if (transaction.amount < 0) {
      sum = sum + transaction.amount;
    }
  });
  return sum;
});
</script>

<template>
  <!-- outer most box -->
  <div class="w-screen h-screen flex justify-center items-center">
    <!-- main card -->
    <div
      class="w-[700px] rounded-md border border-solid border-black p-4 space-y-4"
    >
      <div
        class="grow rounded-md border border-solid border-black divide-y divide-black"
      >
        <div class="p-4 grow">Balance: {{ income + expenses }}</div>
        <div class="flex grow divide-x divide-black">
          <div class="p-4 grow">Income: {{ income }}</div>
          <div class="p-4 grow">Expenses:{{ expenses }}</div>
        </div>
      </div>

      <div class="flex space-x-5">
        <input
          v-model="transactionData.description"
          type="text"
          name="Description"
          id="email"
          class="grow rounded-md border-0 py-1.5 text-gray-900 p-2 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          placeholder="Description"
        />
        <input
          v-model="transactionData.amount"
          type="number"
          name="Amount"
          id="email"
          class="grow rounded-md border-0 py-1.5 text-gray-900 p-2 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          placeholder="Amount"
        />
        <button
          @click="addTransaction"
          type="button"
          class="rounded-md bg-red-600 px-3.5 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
        >
          Add Transaction
        </button>
      </div>
      <Table
        :TransactionProp="Transaction"
        @deleteTransaction="
          (index) => {
            deleteTransaction(index);
          }
        "
      />
    </div>
  </div>
</template>
