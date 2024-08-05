<template>
<Header/>
<div class="Container">
<Balance :total="+total"/>
<IncomeExpense  :income="+income" :expenses="+expenses"/>
<TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDelete"/>
<AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
</div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { useToast } from 'vue-toastification';
import {ref, computed, onMounted} from 'vue';

const toast = useToast();
const transactions = ref([]);

onMounted(()=>{
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
    if(savedTransactions) {
        transactions.value = savedTransactions;
    }
});
// get total
const total = computed(()=>{
    return transactions.value.reduce((acc, transaction)=>{
        return acc + transaction.amount;
    }, 0);
});
// get income
const income = computed(()=>{
    return transactions.value
    .filter((transaction)=> transaction.amount > 0)
    .reduce((acc, transaction)=>{
        return acc + transaction.amount;},0)
        .toFixed(2);
    });

// get expenses
const expenses = computed(()=>{
    return transactions.value
    .filter((transaction)=> transaction.amount < 0)
    .reduce((acc, transaction)=>{
        return acc + transaction.amount;},0)
        .toFixed(2);
    });

    //add transaction
    const handleTransactionSubmitted = (transactionData) => {
        transactions.value.push({
            id:generateUniqueId(),
            text: transactionData.text,
            amount: transactionData.amount
        });
        saveTransactionsToLocalStorage();
        toast.success('transaction added');
    };

    // generate unique id 
    const generateUniqueId =() => {
        return Math.floor(math.random()*1000000);
    }

    //Delete Transaction
    const handleTransactionDelete =(id)=> {
        transactions.value = transactions.value.filter((transaction)=> 
        transaction.id !== id);
        saveTransactionsToLocalStorage();
        toast.success('transaction deleted');
    };

    // save to localStorage
    const saveTransactionsToLocalStorage = () => {
        localStorage.setItem('transactions', JSON.stringify(transactions.value));
    }

</script>