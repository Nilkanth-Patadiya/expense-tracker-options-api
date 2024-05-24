<script>
import Balance from "./components/Balance.vue"
import InfoCard from "./components/InfoCard.vue"
import TransactionList from "./components/TransactionList.vue"
import AddTransaction from "./components/AddTransaction.vue"
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
      transactions: [],
    }
  },
  computed: {
    total() {
      return this.transactions.reduce((acc, t) => acc + t.amount, 0)
    },
    income() {
      return this.transactions
        .filter((t) => t.amount > 0)
        .reduce((acc, t) => acc + t.amount, 0)
    },
    expense() {
      return this.transactions
        .filter((t) => t.amount < 0)
        .reduce((acc, t) => acc + t.amount, 0)
    },
  },
  methods: {
    handleAddTransaction(transactionData) {
      this.transactions.push({
        id: Math.floor(Math.random() * 10000),
        text: transactionData.text,
        amount: parseFloat(transactionData.amount),
      })
      this.saveTransactions()
      this.toast.success("Transaction added")
      console.log(this.transactions)
    },

    handleDeleteTransaction(id) {
      this.transactions = this.transactions.filter((t) => t.id !== id)
      this.saveTransactions()
      this.toast.success("Transaction deleted")
    },

    saveTransactions() {
      localStorage.setItem("transactions", JSON.stringify(this.transactions))
    },
  },
  mounted() {
    var savedTransactions = JSON.parse(localStorage.getItem("transactions"))
    if (savedTransactions) {
      this.transactions = savedTransactions
    }
  },
  components: {
    Balance,
    InfoCard,
    TransactionList,
    AddTransaction,
  },
}
</script>

<template>
  <h2 class="text-center">Expense Tracker</h2>
  <div class="container">
    <Balance :total="total" />
    <InfoCard :income="income" :expense="expense" />
    <TransactionList
      :transactions="transactions"
      @deleteTransaction="handleDeleteTransaction"
    />
    <AddTransaction @addTransaction="handleAddTransaction" />
  </div>
</template>
