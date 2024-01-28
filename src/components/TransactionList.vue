<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in props.transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>{{ transaction.amount }}€</span>
      <button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
    </li>
  </ul>
</template>

<script lang="ts" setup>
import type ITransaction from '../interfaces/ITransaction'

const props = defineProps<{
  transactions: ITransaction[]
}>()

const emit = defineEmits(['transactionDeleted'])

function deleteTransaction(id: string) {
  emit('transactionDeleted', id)
}
</script>
