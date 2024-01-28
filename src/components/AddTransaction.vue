<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" name="text" id="text" placeholder="Enter text..." v-model="text" />
    </div>
    <div class="form-control">
      <label for="amount">
        Amount <br />
        (negative - expense, positive - income)
      </label>
      <input
        type="number"
        step="any"
        name="amount"
        id="amount"
        placeholder="Enter amount..."
        v-model="amount"
      />
    </div>
    <button type="submit" class="btn">Add transaction</button>
  </form>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { useToast } from 'vue-toastification'

const text = ref('')
const amount = ref(0.0)
const toast = useToast()
const emit = defineEmits(['transactionSubmitted'])

function onSubmit() {
  if (text.value.trim() === '' || isNaN(Number(amount.value)) || amount.value == 0) {
    toast.error('Both fields must be filled')
    return
  }

  const transaction = {
    text: text.value.trim(),
    amount: amount.value
  }

  emit('transactionSubmitted', transaction)

  text.value = ''
  amount.value = 0.0
}
</script>
