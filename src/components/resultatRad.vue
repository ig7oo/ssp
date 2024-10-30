<script setup>
import { ref, watch } from 'vue';

const images = {
  win: new URL('@/assets/wincat.png', import.meta.url).href,
  lose: new URL('@/assets/losecat.png', import.meta.url).href,
  tie: new URL('@/assets/tiecat.png', import.meta.url).href,
};

const props = defineProps(['valdaKnappar', 'reset']);
const emit = defineEmits(['vinnare']);
// Const för att hålla resultattext och bildkälla
const resultat = ref('Låt spelet börja!');
const imageSrc = ref('');  

// Övervakar ändringar i props för att uppdatera spelresultatet
watch(props, () => {
  // Kontrollerar om det blev oavgjort
  if (props.valdaKnappar.spelare === props.valdaKnappar.dator) {
    resultat.value = 'Oavgjort!';
    imageSrc.value = images.tie;
  } 
  // Om både spelare och dator har samma paritet (udda eller jämn)
  else if (props.valdaKnappar.spelare % 2 === props.valdaKnappar.dator % 2) {
    if (props.valdaKnappar.spelare > props.valdaKnappar.dator) {
      resultat.value = 'Du vann!';
      imageSrc.value = images.win;
      emit('vinnare', 'spelare');
    } else {
      resultat.value = 'Datorn vann!';
      imageSrc.value = images.lose;
      emit('vinnare', 'dator');
    }
  } 
  // Om spelare och dator har olika paritet
  else {
    if (props.valdaKnappar.spelare < props.valdaKnappar.dator) {
      resultat.value = 'Du vann!';
      imageSrc.value = images.win;
      emit('vinnare', 'spelare');
    } else {
      resultat.value = 'Datorn vann!';
      imageSrc.value = images.lose;
      emit('vinnare', 'dator');
    }
  }
});

// Övervakar reset-prop för att starta om spelet
watch(() => props.reset, () => {
  if (props.reset) {
    resultat.value = 'Låt spelet börja!';
    imageSrc.value = '';  
  }
});
</script>

<template>
  <div class="resultat-container">
    <p :class="{ 'start-text': resultat === 'Låt spelet börja!', 'result-text': resultat !== 'Låt spelet börja!' }">{{ resultat }}</p>   
    <!-- Visar resultatskatten om det finns en bildkälla -->
    <img v-if="imageSrc" :src="imageSrc" alt="Result Image" class="result-image" />
  </div>
</template>

<style scoped>
/* Styling för resultatcontainern */
.resultat-container {
  position: relative;
  padding: 20px;
  text-align: center;
}

/* Texteffekter för start- och resultattext */
.start-text, .result-text {
  color: white;
  text-shadow: -1px -1px 0 #000,
               1px -1px 0 #000,
               -1px 1px 0 #000, 
               1px 1px 0 #000;
}

/* Positionering och storlek för resultatbilden */
.result-image {
  position: fixed;
  bottom: 20px;
  right: 20px;
  height: 100px;
  width: 100px;
}
</style>