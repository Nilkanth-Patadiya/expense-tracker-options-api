<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="t of transactions"
      :key="t.id"
      :class="t.amount < 0 ? 'minus' : 'plus'"
    >
      {{ t.text }} <span>${{ t.amount }}</span
      ><button class="delete-btn" @click="handleClick(t.id)">x</button>
    </li>
  </ul>
</template>
<script>
export default {
  props: {
    transactions: {
      type: Array,
      required: true,
    },
  },
  emits: ["deleteTransaction"],
  methods: {
    handleClick(id) {
      this.$emit("deleteTransaction", id)
    },
  },
}
</script>
