<script setup lang="ts">
import { useToast } from 'vue-toastification'
const emits = defineEmits(['addTransaction']);

const text = ref('');
const amount = ref('');

const toast = useToast();

const onSubmit = () => {
    if (!text.value || !amount.value) {
        toast.error('Text and Amount fields are required');
        return;
    }

    const transactionData = {
        text: text.value,
        amount: parseFloat(amount.value)
    }
    emits('addTransaction', transactionData);
    text.value = '';
    amount.value = '';
}
</script>

<template>
    <h3>Add new transaction</h3>
    <form @submit.prevent="onSubmit" id="form">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" v-model="text" placeholder="Enter text..." />
        </div>
        <div class="form-control">
            <label for="amount">Amount <br />
                (negative - expense, positive - income)</label>
            <input type="text" id="amount" v-model="amount" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>