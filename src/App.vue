<script setup>
 import { ref } from 'vue';
 import knappRad from './components/knappRad.vue';
 import resultatRad from './components/resultatRad.vue';
 import poangRad from './components/poangRad.vue';

 const resultat = ref({})
 const knappar = ref (['Sten', 'Sax', 'Påse']);
 const vinnare = ref('');
 const reset = ref(true)
 const nyttVal = ref('')

function hittaVinnare(valdaKnappar) {
  reset.value = false
  vinnare.value=''
    let spelare = knappar.value.indexOf(valdaKnappar.spelare)
    let dator = knappar.value.indexOf(valdaKnappar.dator)
    resultat.value = {spelare: spelare, dator: dator}
    }

function raknaPoang(v) {
  vinnare.value = v
}

function skapaVal() {
  if (nyttVal.value.trim()) {
    knappar.value.push(nyttVal.value)
    nyttVal.value = ''
  }
}

function undoVal() {
  if (knappar.value.length > 3) {
    knappar.value.pop()
  }
}

</script>

<template>
  <header>
    <h1>Sten, sax, påse!</h1>
  </header>

  <main>
    <div class="nytt-val">
      <input v-model="nyttVal" placeholder="Skriv nytt val" />
      <br><br>
      <button @click="skapaVal">Lägg till val</button><br><br>
      <button @click="undoVal">Ångra senaste val</button>
    </div>
    <knappRad :knappar="knappar" @valda-knappar="hittaVinnare" :reset="reset"/>
    <resultatRad :valdaKnappar="resultat" @vinnare="raknaPoang" :reset="reset"/>
    <poangRad :vinnare="vinnare" :reset="reset"/>
    <div class="score">
      <button id="nolla" @click="reset = true">Nollställ</button>
    </div>
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

   .nytt-val {
    text-align: center;
    margin-bottom: 1em;
   }

   input {
    padding: .6em;
    font-size: 1em;
    margin-right: .5em;
    border: 1px solid #cccc;
    border-radius: 5px;
   }
</style>
