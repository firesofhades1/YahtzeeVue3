<script setup>
import { ref, computed } from 'vue';

const props = defineProps(["rollArray"]);
const emit = defineEmits(["update:rollArray"])

const diceIcons = { 1: "&#x2680;", 2: "&#x2681;", 3: "&#x2682;", 4: "&#x2683;", 5: "&#x2684;", 6: "&#x2685;" };

props.rollArray = [1,2,3,4,5];

const rollIcons = computed(() => {
    return [
        diceIcons[props.rollArray[0]],
        diceIcons[props.rollArray[1]],
        diceIcons[props.rollArray[2]],
        diceIcons[props.rollArray[3]],
        diceIcons[props.rollArray[4]]
    ];
});

const count = ref([
    { id: 1, amount: 0 },
    { id: 2, amount: 0 },
    { id: 3, amount: 0 },
    { id: 4, amount: 0 },
    { id: 5, amount: 0 },
    { id: 6, amount: 0 }
]);



const ThrowDice = () => {
    count.value.forEach((element) => {
        element.amount = 0;
    })
    rollArray.values = [];

    for (let i = 0; i < 5; i++) {
        const roll = Math.ceil(Math.random() * 6);
        rollArray.values.push(roll);
        count.value[roll - 1].amount++;
    }
}





</script>

<template>
    <div class="fiveDice">
        <button id="dice1" class="dice textHover" v-html=rollIcons[0]></button>
        <button id="dice2" class="dice textHover" v-html=rollIcons[1]></button>
        <button id="dice3" class="dice textHover" v-html=rollIcons[2]></button>
        <button id="dice4" class="dice textHover" v-html=rollIcons[3]></button>
        <button id="dice5" class="dice textHover" v-html=rollIcons[4]></button>
    </div>

    <div class="button">
        <button @click="ThrowDice" id="btn" class="btn wHover">Gooien</button>
        <button id="btnFill" class="btn wHover">Vul score in</button>
    </div>
</template>