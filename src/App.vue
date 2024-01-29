<script setup>
import Table from "./components/Table.vue";
import { ref, computed, watchEffect } from "vue";
import gsap from "gsap";

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

// Animation logic
const number = ref(0);

watchEffect(() => {
  gsap.to(number, { duration: 0.5, value: income.value + expenses.value });
});
</script>

<template>
  <div
    class="min-h-screen flex justify-center items-center p-4 bg-gradient-to-r from-gray-900 to-slate-800"
  >
    <div
      class="w-[400px] max-w-screen-md mx-auto rounded-md border border-solid p-4 space-y-4 bg-white"
    >
      <div class="justify-center items-center flex font-extrabold text-red-600">
        <h1>EXPENSE TRACKER</h1>
      </div>
      <div
        class="grow rounded-md border border-solid border-black divide-y divide-black"
      >
        <div class="grow p-4">Balance: {{ number }}</div>
        <div class="grow flex divide-x divide-black">
          <div class="grow p-4">Income: {{ income }}</div>
          <div class="grow p-4">Expenses: {{ expenses }}</div>
        </div>
      </div>

      <div class="flex flex-col space-y-4">
        <input
          v-model="transactionData.description"
          type="text"
          name="Description"
          class="rounded-md border-0 py-2 px-3 text-gray-900 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm"
          placeholder="Description"
        />
        <input
          v-model="transactionData.amount"
          type="number"
          name="Amount"
          class="rounded-md border-0 py-2 px-3 text-gray-900 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm"
          placeholder="Amount"
        />
        <button
          @click="addTransaction"
          type="button"
          :disabled="!transactionData.description || !transactionData.amount"
          class="rounded-md bg-red-600 px-4 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500"
        >
          Add Transaction
        </button>
      </div>

      <Table
        v-if="Transaction.length > 0"
        :TransactionProp="Transaction"
        @deleteTransaction="(index) => deleteTransaction(index)"
      />
    </div>
  </div>
</template>
