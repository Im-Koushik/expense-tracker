<script setup lang="ts">
import { Transaction } from '@/types/transaction';
import { v4 as uuidv4 } from 'uuid';
import { useToast } from 'vue-toastification';

const toast = useToast();
const transactions = ref<Transaction[]>([])

const total = computed(() => {
    return transactions.value.reduce((total, transaction) => {
        return total + transaction.amount
    }, 0)
})

const income = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((total, transaction) => {
            return total + transaction.amount
        }, 0)
        .toFixed(2)
})

const expenses = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((total, transaction) => {
            return total + transaction.amount
        }, 0)
        .toFixed(2)
})

const generateUniqueId = () => {
    return uuidv4();
}

const handleAddTransaction = (transactionData: Transaction) => {
    transactions.value.push({
        id: generateUniqueId(),
        text: transactionData.text,
        amount: transactionData.amount
    })
    saveToLocalStorage();
    toast.success('Transaction added successfully')
}

const handleDeleteTransaction = (id: string) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id)
    saveToLocalStorage();
    toast.success('Transaction deleted successfully')
}

//Save to Local Storage
const saveToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

onMounted(() => {
    const transactionsFromLocalStorage = JSON.parse(localStorage.getItem('transactions') || '[]')
    transactions.value = transactionsFromLocalStorage
})

onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions') || '[]');
    if (savedTransactions) transactions.value = savedTransactions
})
</script>

<template>
    <Header />
    <div class="container">
        <Balance :total="+total" />
        <Expense :income="+income" :expenses="+expenses" />
        <TransactionList :transactions="transactions" @deleteTransaction="handleDeleteTransaction" />
        <AddTransaction @addTransaction="handleAddTransaction" />
    </div>
</template>