<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="handleSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        v-model="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
<script>
import { useToast } from "vue-toastification"

export default {
  setup() {
    // Get toast interface
    const toast = useToast()
    // Make it available inside methods
    return { toast }
  },
  data() {
    return {
      amount: "",
      text: "",
    }
  },
  emits: ["addTransaction"],
  methods: {
    handleSubmit() {
      if (!this.text || !this.amount) {
        this.toast.error("Any field shouldn't be empty")
        return
      }

      this.$emit("addTransaction", {
        text: this.text,
        amount: this.amount,
      })

      this.text = ""
      this.amount = ""
    },
  },
}
</script>
