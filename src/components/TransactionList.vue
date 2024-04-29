<script setup lang="ts">
import { Transaction } from '@/types/transaction';

const props = defineProps<{
    transactions: Transaction[]
}>();

const emits = defineEmits(['deleteTransaction'])

const deleteTransaction = (id: string) => {
    emits('deleteTransaction', id)
}
</script>

<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions" :key="transaction.id"
            :class="{ 'minus': transaction.amount < 0, 'plus': transaction.amount > 0 }">
            {{ transaction.text }} <span>{{ transaction.amount }}</span><button
                @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
    </ul>
</template>