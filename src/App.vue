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

// Funktion för att lägga till nya val
function skapaVal() {
  if (nyttVal.value.trim()) {
    if (nyttVal.value.length <= 20) {
      if (!knappar.value.includes(nyttVal.value)) {
        knappar.value.push(nyttVal.value)
        nyttVal.value = ''
      } else {
        alert('Detta val finns redan!')
      }
    } else {
      alert('Ditt val kan inte vara längre än 20 bokstäver!')
    }
  }
}
// Funktion för att ta bort senaste val
function undoVal() {
  if (knappar.value.length > 3) {
    knappar.value.pop()
  } else {
    alert('Du kan inte ta bort Sten, Sax eller Påse!')
  }
}

</script>

<template>
  <header>
    <h1>Sten, sax, påse + vad du vill!</h1>
  </header>

  <main>
    <div class="nytt-val">
      <input v-model="nyttVal" placeholder="Skriv nytt val" />
      <br><br>
      <button id="flerval" @click="skapaVal">Lägg till val</button><br><br>
      <button id="raderaval" @click="undoVal">Ångra senaste val</button>
    </div>
    <knappRad :knappar="knappar" @valda-knappar="hittaVinnare" :reset="reset"/>
    <resultatRad :valdaKnappar="resultat" @vinnare="raknaPoang" :reset="reset"/>
    <poangRad :vinnare="vinnare" :reset="reset"/>
    <div class="score">
      <button id="nolla" @click="reset = true">Nollställ poäng</button>
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
    background-color:#ffffffc9;
    border: 1px solid #000000cc;
    border-radius: 5px;
    cursor: pointer;
   }

   #raderaval:hover {
    background-color: #ff6b6b;
    transition: background-color 0.3s;
   }

   #flerval:hover {
    background-color: #90EE90;
    transition: background-color 0.3s;
   }

   .knapprad {
    display: flex;
    justify-content: center;
    gap: .6em;
    flex-wrap: wrap;
    max-width: 800px;
    margin: 0 auto;
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
    border: rgb(255, 0, 0) solid 2px;
   }
   #nolla {
    margin-top: 2em;
    padding: .3em .6em;
    font-size: .8em;
   }

   #nolla:hover {
    background-color: #ffd900ad;
    transition: background-color 0.3s;
   }

   .nytt-val {
    text-align: center;
    margin-bottom: 1em;
   }

   input {
    padding: .6em;
    font-size: 1em;
    margin-right: .5em;
    border: 1px solid #000000;
    border-radius: 5px;
   }
   h1 {
    font-size: 39px;
    margin-bottom: 1em;
    color: #ffffff;
    -webkit-text-stroke: 1px rgb(0, 0, 0); 
   }
</style>