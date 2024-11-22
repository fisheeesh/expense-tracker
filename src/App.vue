<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expenses="+expenses" />
    <TransactionList @transactionDeleted="handleTranscationDeleted" :transactions="transactions" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import { computed, onMounted, ref, watch } from 'vue';
import AddTransaction from './components/AddTransaction.vue';
import Balance from './components/Balance.vue';
import Header from './components/Header.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import { useToast } from 'vue-toastification';

const toast = useToast()

const transactions = ref(
  [
    // { id: 1, text: 'Flower', amount: -19.99 },
    // { id: 2, text: 'Salary', amount: 299.97 },
    // { id: 3, text: 'Book', amount: -10 },
    // { id: 4, text: 'Camera', amount: 150 }
  ]
)

//Get total
const total = computed(() => transactions.value.reduce((acc, transaction) => acc + transaction.amount, 0))

//Get Incomes
const income = computed(() => transactions.value
  .filter(transaction => transaction.amount > 0)
  .reduce((acc, transaction) => acc + transaction.amount, 0)
  .toFixed(2)
)

//Get Expenses
const expenses = computed(() => transactions.value
  .filter(transaction => transaction.amount < 0)
  .reduce((acc, transaction) => acc + transaction.amount, 0)
  .toFixed(2)
)

//Handle transaction submitted
const handleTransactionSubmitted = (data) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: data.text,
    amount: data.amount
  })
  // console.log(generateUniqueId())
  saveTransactionsToLocalStorage()
  toast.success('Transaction added successfully')
}

//Generate unique id for each new transaction
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000)
}

//Delete specific transaction with id
const handleTranscationDeleted = (id) => {
  // console.log(id)
  transactions.value = transactions.value.filter(transaction => transaction.id !== id)
  saveTransactionsToLocalStorage()
  toast.success('Transaction deleted successfully')
}

onMounted(() => {
  const saveTransactions = JSON.parse(localStorage.getItem('transactions'))

  if (saveTransactions) {
    transactions.value = saveTransactions
  }
})

//Save to local storage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
// watch(transactions, () => {
//   localStorage.setItem('transactions', JSON.stringify(transactions.value))
// })

</script>

<style></style>