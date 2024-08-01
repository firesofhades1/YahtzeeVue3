<script setup>
import ThrownDice from './components/ThrownDice.vue'
import ScoreTale from "./components/ScoreTable.vue"
let rolls = [];
</script>

<template>
 <div id="container">
        <header>
            <h1>Yathzee</h1>
        </header>

        <ThrownDice :rollArray="rolls"/>

        <div class="round">
            <p id="rounds"></p>
        </div>

        <div class="howTo">
            <p>Klik <span class="borderBtn">Gooien</span> om de dobbelstenen te gooien, klik op een dobbelsteen om de waarde vast te houden.</p>
            <p>Na drie worpen klik je op <span class="borderBtn">Vul score in</span> om de score in te vullen.</p>
            <p>Kies welke score je wil vastleggen op het scoreblok.</p>
        </div>

        <ScoreTale/>
        
    </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
