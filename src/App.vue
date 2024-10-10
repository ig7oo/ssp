<script setup>
 import { ref } from 'vue';
 import knappRad from './components/knappRad.vue';
 import resultatRad from './components/resultatRad.vue';
 import poangRad from './components/poangRad.vue';

 const score = ref ({ spelare: 0, dator: 0});
 const resultat = ref({})
 const knappar = ref (['Sten', 'Sax', 'Påse', 'Lizard', 'Spock']);
 const vinnare = ref('');

function hittaVinnare(valdaKnappar) {
  vinnare.value=''
    let spelare = knappar.value.indexOf(valdaKnappar.spelare)
    let dator = knappar.value.indexOf(valdaKnappar.dator)
    resultat.value = {spelare: spelare, dator: dator}
    }

    function reset() {
    score.value.spelare = 0
    score.value.dator = 0
    resultat.value = 'Låt spelet börja!'
    let buttons = document.getElementsByClassName('alternativ')
    for (let b of buttons) {
        b.classList.remove('spelarval')
        b.classList.remove('datorval')
    }
}

function raknaPoang(v) {
  vinnare.value = v
}
</script>

<template>
  <header>
    <h1>Sten, sax, påse!</h1>
  </header>

  <main>
    <knappRad :knappar="knappar" @valda-knappar="hittaVinnare"/>
    <resultatRad :valdaKnappar="resultat" @vinnare="raknaPoang"/>
    <poangRad :vinnare="vinnare"/>
    
    
  </main>
</template>

<style scoped>
   header {
        text-align: center;
        margin-bottom: 1.2em;
   }

   button {
    padding: .6em 1.2em;
    font-size: 1.2em;
    background-color:#f0f0f0;
    border: 1px solid #cccc;
    border-radius: 5px;
    cursor: pointer;
   }

   .knapprad {
    display: flex;
    justify-content: center;
    gap: .6em;
   }

   .resultat {
    font-size: 1.2em;
    text-align: center;
    margin: 1.2em 0;
   }

   .score {
    font-size: 1.2em;
    text-align: center;
   }
   
   button.spelarval {
    background-color: greenyellow;
   }
   button.datorval {
    border: red solid 2px;
   }
   #nolla {
    margin-top: 2em;
    padding: .3em .6em;
    font-size: .8em;
   }
</style>
