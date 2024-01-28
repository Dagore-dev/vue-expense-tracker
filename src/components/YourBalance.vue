<template>
  <h4>Your balance</h4>
  <h1 id="balance">{{ props.total }}€</h1>
</template>

<script lang="ts" setup>
const props = defineProps<{
  total: string
}>()
</script>
