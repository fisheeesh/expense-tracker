<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" placeholder="Enter text..." v-model="form.text" />
        </div>
        <div class="form-control">
            <label for="amount">Amount <br />
                (negative - expense, positive - income)</label>
            <input type="text" id="amount" placeholder="Enter amount..." v-model="form.amount" />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>

<script setup>
import { reactive } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

const emit = defineEmits( ['transactionSubmitted'])

const form = reactive({
    text: null,
    amount: null
})

const onSubmit = () => {
    
    if(!form.text || !form.amount){
        toast.error('Both fields must be filled')
        return;
    }
    // console.log(form.text, form.amount)
    form.text = ''
    form.amount = ''
}

</script>

<style></style>