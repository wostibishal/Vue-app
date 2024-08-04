<template>
    <h3>
        History
    </h3>
    <ul id="list" class="list">
        <!-- <li class="minus"> -->
            <!-- cash<span>-$400</span> -->
            <!-- <button class="delete-btn">x</button> -->
        <!-- </li> -->
        <!-- <li class="plus"> -->
        <!-- paycheck<span>$400</span> -->
        <!-- <button class="delete-btn">x</button> -->
        <!-- </li> -->
    </ul>
</template>

<script>
export default{
    data(){
        return{
            transactions: [
                {id:1, text: ''}
            ]
        }
    }
}
</script>