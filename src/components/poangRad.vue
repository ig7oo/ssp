<script setup>
import { ref, watch } from 'vue';

// Definierar props som komponenten tar emot: knappar, reset och vinnare
const props = defineProps(['knappar', 'reset', 'vinnare'])


// Skapar ett reaktivt objekt för att hålla reda på poängen
const score = ref({ spelare: 0, dator: 0});

// Övervakar ändringar i props och uppdaterar poängen när någon vinner
watch(props, () => {
    if (props.vinnare == 'spelare') {
        score.value.spelare++
    } else if(props.vinnare==='dator'){ 
        score.value.dator++
    }
})

// Övervakar reset-prop och nollställer poängen när reset är true
watch(
  () => props.reset,
  () => {
    if (props.reset) {
      score.value.spelare = 0
      score.value.dator = 0
    }
  }
)
</script>

<template>
    <!-- Visar poängställningen i formatet Spelare:Dator -->
    <div class="score">
        <p>
            <span id="spelare" class="player">
                {{ score.spelare }}
            </span>:<span id="dator" class="computer">
                {{ score.dator }}
            </span>
        </p>
    </div>
</template>

<style scoped>
/* Styling för poängdisplayen */
.score {
    font-size: 35px;
    color: rgb(255, 255, 255);
    text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000, 1px 1px 0 #000;
}

/* Grön färg för spelarens poäng */
.player {
    color: #00ff00;
}

/* Röd färg för datorns poäng */
.computer {
    color: #ff0000;
}
</style>