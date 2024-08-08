<script setup>
import { ref, computed } from 'vue';


const smallMask = 0b001111;
const largeMask = 0b011111;

const props = defineProps({ newCount: Array });

const top = ref(computed(() => {
    return {
        "ones": props.newCount[0].amount * 1,
        "twos": props.newCount[1].amount * 2,
        "threes": props.newCount[2].amount * 3,
        "fours": props.newCount[3].amount * 4,
        "fives": props.newCount[4].amount * 5,
        "sixes": props.newCount[5].amount * 6
    }
}));

const total = computed(() => {
    return top.value.ones +
        top.value.twos +
        top.value.threes +
        top.value.fours +
        top.value.fives +
        top.value.sixes;
});
const bonus = computed(() => {
    return total.value > 63 ? 35 : 0;
});
const total1 = computed(() => {
    return total.value + bonus.value;
});

const threeKindScore = computed(() => {
    return countCheck(3) ? total : 0;
});

const fourKindScore = computed(() => {
    return countCheck(4) ? total : 0;
});

const fullHouseScore = computed(() => {
    return countCheck(2, "exact") && countCheck(3, "exact") ? 25 : 0;
});

const yahtzeeScore = computed(() => {
    return countCheck(5) ? 50 : 0;
});

const chanceScore = computed(() => {
    return total.value;
});

const smallStraightScore = computed(() => {
    const flags = getStraightFlags();
    return bitMatch(flags, smallMask, 2) ? 30 : 0;
})
      
const largeStraightScore = computed(() => {
    const flags = getStraightFlags();
    return bitMatch(flags, largeMask, 1) ? 40 : 0;
})

const total2 = computed(() => {
    return threeKindScore.value +
        fourKindScore.value +
        fullHouseScore.value +
        smallStraightScore.value +
        largeStraightScore.value +
        yahtzeeScore.value +
        chanceScore.value;
});

const finalScore = computed(() => {
    return total1.value + total2.value;
});

const countCheck = (amount, mode = "default") => {
    let result = false;
    props.newCount.forEach(element => {
        switch (mode) {
            case "default":
                if (element.amount >= amount) result = true;
                break;
            case "exact":
                if (element.amount === amount) result = true;
                break;
        }
    })
    return result;
}

const getStraightFlags = () => {
    const flag = [0, 1, 2, 4, 8, 16, 32];
    let straightFlags = 0b000000;
    
    for (let i = 0; i < 6; i++) {
        if(props.newCount[i].amount > 0) straightFlags += flag[i];  
    }
    return straightFlags;
}

const bitMatch = (flags, mask, shift) => {
    if (shift < 0) return false;
    if ((flags & (mask << shift)) === mask << shift) {
        return true;
    }
    return bitMatch(flags, mask, shift - 1);
}
</script>


<template>

    <div class="paper">
        <table>
            <tr>
                <td class="noBorder"></td>
            </tr>
            <tr>
                <th class="title noBorder" colspan="7">scoreblok</th>
                <th class="title noBorder"><img class="posImg" src="/images/dice.png"></th>
            </tr>
            <tr>
                <td class="noBorder"></td>
            </tr>
            <tr>
                <th class="h1 noBorder" colspan="2">deel 1</th>
                <th class="h1 noBorder">hoe te scoren</th>
                <th class="h2 noBorder">Spel 1</th>
                <th class="h2 noBorder">Spel 2</th>
                <th class="h2 noBorder">Spel 3</th>
                <th class="h2 noBorder">Spel 4</th>
                <th class="h2 noBorder">Spel 5</th>
            </tr>
            <tr>
                <td class="noBorderRight">enen</td>
                <td class="dicePreview noBorderLeft">&#x2680;</td>
                <td class="howToScore">waarde van alle enen</td>
                <td id="aces1" class="fillFont keepScore">{{ top.ones }}</td>
                <td id="aces2" class="fillFont keepScore"></td>
                <td id="aces3" class="fillFont keepScore"></td>
                <td id="aces4" class="fillFont keepScore"></td>
                <td id="aces5" class="fillFont keepScore"></td>
            </tr>
            <tr>
                <td class="noBorderRight">tweeën</td>
                <td class="dicePreview noBorderLeft">&#x2681;</td>
                <td class="howToScore">waarde van alle tweeën</td>
                <td id="twos1" class="fillFont keepScore">{{ top.twos }}</td>
                <td id="twos2" class="fillFont keepScore"></td>
                <td id="twos3" class="fillFont keepScore"></td>
                <td id="twos4" class="fillFont keepScore"></td>
                <td id="twos5" class="fillFont keepScore"></td>
            </tr>
            <tr>
                <td class="noBorderRight">drieën</td>
                <td class="dicePreview noBorderLeft">&#x2682;</td>
                <td class="howToScore">waarde van alle drieën</td>
                <td id="threes1" class="fillFont keepScore">{{ top.threes }}</td>
                <td id="threes2" class="fillFont keepScore"></td>
                <td id="threes3" class="fillFont keepScore"></td>
                <td id="threes4" class="fillFont keepScore"></td>
                <td id="threes5" class="fillFont keepScore"></td>
            </tr>
            <tr>
                <td class="noBorderRight">vieren</td>
                <td class="dicePreview noBorderLeft">&#x2683;</td>
                <td class="howToScore">waarde van alle vieren</td>
                <td id="fours1" class="fillFont keepScore">{{ top.fours }}</td>
                <td id="fours2" class="fillFont keepScore"></td>
                <td id="fours3" class="fillFont keepScore"></td>
                <td id="fours4" class="fillFont keepScore"></td>
                <td id="fours5" class="fillFont keepScore"></td>
            </tr>
            <tr>
                <td class="noBorderRight">vijven</td>
                <td class="dicePreview noBorderLeft">&#x2684;</td>
                <td class="howToScore">waarde van alle vijven</td>
                <td id="fives1" class="fillFont keepScore">{{ top.fives }}</td>
                <td id="fives2" class="fillFont keepScore"></td>
                <td id="fives3" class="fillFont keepScore"></td>
                <td id="fives4" class="fillFont keepScore"></td>
                <td id="fives5" class="fillFont keepScore"></td>
            </tr>
            <tr>
                <td class="noBorderRight">zessen</td>
                <td class="dicePreview noBorderLeft">&#x2685;</td>
                <td class="howToScore">waarde van alle zessen</td>
                <td id="sixes1" class="fillFont keepScore">{{ top.sixes }}</td>
                <td id="sixes2" class="fillFont keepScore"></td>
                <td id="sixes3" class="fillFont keepScore"></td>
                <td id="sixes4" class="fillFont keepScore"></td>
                <td id="sixes5" class="fillFont keepScore"></td>
            </tr>
            <tr id="total">
                <td class="uppercase" colspan="2">totaal</td>
                <td class="arrow">&#10132;</td>
                <td id="upperScore1" class="fillFont">{{ total }}</td>
                <td id="upperScore2" class="fillFont"></td>
                <td id="upperScore3" class="fillFont"></td>
                <td id="upperScore4" class="fillFont"></td>
                <td id="upperScore5" class="fillFont"></td>
            </tr>
            <tr id="bonus">
                <td class="noBorderRight">bonus</td>
                <td class="alt noBorderLeft">als totaal 63<br>of meer is</td>
                <td class="howToScore">35 punten</td>
                <td id="bonus1" class="fillFont">{{ bonus }}</td>
                <td id="bonus2" class="fillFont"></td>
                <td id="bonus3" class="fillFont"></td>
                <td id="bonus4" class="fillFont"></td>
                <td id="bonus5" class="fillFont"></td>
            </tr>
            <tr id="upperTotal">
                <td class="uppercase noBorderRight">totaal</td>
                <td class="alt uppercase noBorderLeft">van de<br>bovenste<br>helft</td>
                <td class="arrow">&#10132;</td>
                <td id="totalUpperScore1" class="fillFont">{{ total1 }}</td>
                <td id="totalUpperScore2" class="fillFont"></td>
                <td id="totalUpperScore3" class="fillFont"></td>
                <td id="totalUpperScore4" class="fillFont"></td>
                <td id="totalUpperScore5" class="fillFont"></td>
            </tr>
            <tr>
                <th class="h1 noBorder" colspan="2">deel 2</th>
            </tr>

            <tr>
                <td colspan="2">3 of a kind</td>
                <td class="howToScore">totaal van alle dobbelstenen</td>
                <td id="threeOfKind1" class="fillFont keepScore">{{ threeKindScore }}</td>
                <td id="threeOfKind2" class="fillFont keepScore"></td>
                <td id="threeOfKind3" class="fillFont keepScore"></td>
                <td id="threeOfKind4" class="fillFont keepScore"></td>
                <td id="threeOfKind5" class="fillFont keepScore"></td>
            </tr>
            <tr>
                <td colspan="2">4 of a kind</td>
                <td class="howToScore">totaal van alle dobbelstenen</td>
                <td id="fourOfKind1" class="fillFont keepScore">{{ fourKindScore }}</td>
                <td id="fourOfKind2" class="fillFont keepScore"></td>
                <td id="fourOfKind3" class="fillFont keepScore"></td>
                <td id="fourOfKind4" class="fillFont keepScore"></td>
                <td id="fourOfKind5" class="fillFont keepScore"></td>
            </tr>
            <tr>
                <td colspan="2">full house</td>
                <td class="howToScore">25 punten</td>
                <td id="fullHouse1" class="fillFont keepScore">{{ fullHouseScore }}</td>
                <td id="fullHouse2" class="fillFont keepScore"></td>
                <td id="fullHouse3" class="fillFont keepScore"></td>
                <td id="fullHouse4" class="fillFont keepScore"></td>
                <td id="fullHouse5" class="fillFont keepScore"></td>
            </tr>
            <tr>
                <td class="noBorderRight">kleine straat</td>
                <td class="alt noBorderLeft">4<br>opeenvolgende nummers</td>
                <td class="howToScore">30 punten</td>
                <td id="smStraight1" class="fillFont keepScore">{{ smallStraightScore }}</td>
                <td id="smStraight2" class="fillFont keepScore"></td>
                <td id="smStraight3" class="fillFont keepScore"></td>
                <td id="smStraight4" class="fillFont keepScore"></td>
                <td id="smStraight5" class="fillFont keepScore"></td>
            </tr>
            <tr>
                <td class="noBorderRight">grote straat</td>
                <td class="alt noBorderLeft">5<br>opeenvolgende nummers</td>
                <td class="howToScore">40 punten</td>
                <td id="lgStraight1" class="fillFont keepScore">{{ largeStraightScore }}</td>
                <td id="lgStraight2" class="fillFont keepScore"></td>
                <td id="lgStraight3" class="fillFont keepScore"></td>
                <td id="lgStraight4" class="fillFont keepScore"></td>
                <td id="lgStraight5" class="fillFont keepScore"></td>
            </tr>
            <tr>
                <td class="noBorderRight">yahtzee</td>
                <td class="alt noBorderLeft">5<br>dezelfde<br>nummers</td>
                <td class="howToScore">50 punten</td>
                <td id="yahtzee1" class="fillFont keepScore">{{ yahtzeeScore }}</td>
                <td id="yahtzee2" class="fillFont keepScore"></td>
                <td id="yahtzee3" class="fillFont keepScore"></td>
                <td id="yahtzee4" class="fillFont keepScore"></td>
                <td id="yahtzee5" class="fillFont keepScore"></td>
            </tr>
            <tr id="chance">
                <td colspan="2">chance</td>
                <td class="howToScore">totaal van alle dobbelstenen</td>
                <td id="chance1" class="fillFont keepScore">{{ chanceScore }}</td>
                <td id="chance2" class="fillFont keepScore"></td>
                <td id="chance3" class="fillFont keepScore"></td>
                <td id="chance4" class="fillFont keepScore"></td>
                <td id="chance5" class="fillFont keepScore"></td>
            </tr>
            <tr id="lowerTotal">
                <td class="uppercase noBorderRight">totaal</td>
                <td class="alt uppercase noBorderLeft">van de<br>onderste<br>helft</td>
                <td class="arrow">&#10132;</td>
                <td id="lowerScore1" class="fillFont">{{ total2 }}</td>
                <td id="lowerScore2" class="fillFont"></td>
                <td id="lowerScore3" class="fillFont"></td>
                <td id="lowerScore4" class="fillFont"></td>
                <td id="lowerScore5" class="fillFont"></td>
            </tr>
            <tr id="lowUpperTotal">
                <td class="uppercase noBorderRight">totaal</td>
                <td class="alt uppercase noBorderLeft">van de<br>bovenste<br>helft</td>
                <td class="arrow">&#10132;</td>
                <td id="upperScoreLower1" class="fillFont">{{ total1 }}</td>
                <td id="upperScoreLower2" class="fillFont"></td>
                <td id="upperScoreLower3" class="fillFont"></td>
                <td id="upperScoreLower4" class="fillFont"></td>
                <td id="upperScoreLower5" class="fillFont"></td>
            </tr>
            <tr id="grandTotal">
                <td class="bold" colspan="2">totaal generaal</td>
                <td class="arrow">&#10132;</td>
                <td id="grandTotal1" class="fillFont">{{ finalScore }}</td>
                <td id="grandTotal2" class="fillFont"></td>
                <td id="grandTotal3" class="fillFont"></td>
                <td id="grandTotal4" class="fillFont"></td>
                <td id="grandTotal5" class="fillFont"></td>
            </tr>
        </table>
    </div>
</template>