<template>
  <AppHeader />
  <div class="container">
    <YourBalance :total="total" />
    <IncomeExpenses :income="income" :expense="expense" />
    <TransactionList :transactions="transactions" @transaction-deleted="handleTransactionDeleted" />
    <AddTransaction @transaction-submitted="handleTransactionSubmitted" />
  </div>
</template>

<script lang="ts" setup>
import { computed, onMounted, ref } from 'vue'
import { useToast } from 'vue-toastification'
import AddTransaction from './components/AddTransaction.vue'
import AppHeader from './components/AppHeader.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import YourBalance from './components/YourBalance.vue'
import type ITransaction from './interfaces/ITransaction'

const transactions = ref<ITransaction[]>([])
const toast = useToast()

onMounted(() => {
  transactions.value = JSON.parse(localStorage.getItem('transactions') ?? '[]')
})

const total = computed<string>(() => {
  return transactions.value
    .reduce((accumulator, current) => accumulator + current.amount, 0)
    .toFixed(2)
})

const income = computed<string>(() => {
  return transactions.value
    .filter((t) => t.amount > 0)
    .reduce((accumulator, current) => accumulator + current.amount, 0)
    .toFixed(2)
})

const expense = computed<string>(() => {
  return transactions.value
    .filter((t) => t.amount < 0)
    .reduce((accumulator, current) => accumulator + current.amount, 0)
    .toFixed(2)
})

function handleTransactionSubmitted(transaction: { text: string; amount: number }) {
  transactions.value.push({
    id: crypto.randomUUID(),
    ...transaction
  })

  saveTransactionsToLocalStorage()
  toast.success('Transaction added')
}

function handleTransactionDeleted(id: string) {
  transactions.value = transactions.value.filter((t) => t.id !== id)

  saveTransactionsToLocalStorage()
  toast.success('Transaction deleted')
}

function saveTransactionsToLocalStorage() {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>
