<script setup>
import AddTransaction from "./components/AddTransaction.vue";
import Balance from "./components/Balance.vue";
import Header from "./components/Header.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import TransactionList from "./components/TransactionList.vue";
import { computed, ref } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();
const transactions = ref([]);
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(3);
});
//Add Transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  toast.success("Transaction Added");
};
//transaction delete
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id!== id
  );
  toast.success("Transaction Deleted");
};
//Generate id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};
</script>

<template>
  <div class="container">
    <h1>My App</h1>
    <Header />
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expenses="+expenses" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
    />
    <AddTransaction @TransactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>
