<script setup>
 import { ref } from 'vue';
 import knappRad from './components/knappRad.vue';
 import resultatRad from './components/resultatRad.vue';
 import poangRad from './components/poangRad.vue';

 const resultat = ref({})  // Lagrar spelarens och datorns val
 const knappar = ref (['Sten', 'Sax', 'Påse']);  // Lista med tillgängliga val
 const vinnare = ref('');  // Lagrar vem som vann omgången
 const reset = ref(true)   // Kontrollerar om spelet ska återställas
 const nyttVal = ref('')   // Lagrar användarens input för nya val

// Funktion som hanterar val och bestämmer vinnare
function hittaVinnare(valdaKnappar) {
  reset.value = false
  vinnare.value=''
    let spelare = knappar.value.indexOf(valdaKnappar.spelare)
    let dator = knappar.value.indexOf(valdaKnappar.dator)
    resultat.value = {spelare: spelare, dator: dator}
    }

// Funktion som uppdaterar vinnaren och poängställningen
function raknaPoang(v) {
  vinnare.value = v
}

// Funktion för att lägga till nya val
function skapaVal() {
  if (nyttVal.value.trim()) {  // Kontrollera att input inte är tomt
    if (nyttVal.value.length <= 20) {  // Kontrollerar att längden inte överstiger 20 tecken
      if (!knappar.value.includes(nyttVal.value)) {  // Kontrollera att valet inte redan finns
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

// Funktion för att ta bort det senast tillagda valet
function undoVal() {
  if (knappar.value.length > 3) {  // Kontrollera att Sten, Sax och Påse inte kan tas bort    
    knappar.value.pop() // pop() tar bort och returnerar det sista elementet i en array
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
      <input id="nyttValInput" name="nyttValInput" v-model="nyttVal" placeholder="Skriv nytt val" />
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
   /* Stilar för header och huvudrubrik */
   header {
        text-align: center;
        margin-bottom: 1.2em;
   }

   /* Grundläggande knappstil */
   button {
    padding: .6em 1.2em;
    font-size: 1.2em;
    background-color:#ffffffc9;
    border: 1px solid #000000cc;
    border-radius: 5px;
    cursor: pointer;
   }

   /* Hover-effekter för knappar */
   #raderaval:hover {
    background-color: #ff6b6b;
    transition: background-color 0.3s;
   }

   #flerval:hover {
    background-color: #90EE90;
    transition: background-color 0.3s;
   }

   /* Layout för knapprad */
   .knapprad {
    display: flex;
    justify-content: center;
    gap: .6em;
    flex-wrap: wrap;
    max-width: 800px;
    margin: 0 auto;
   }

   /* Stilar för resultat och poängvisning */
   .resultat {
    font-size: 1.2em;
    text-align: center;
    margin: 1.2em 0;
   }

   .score {
    font-size: 1.2em;
    text-align: center;
   }
   
   /* Stilar för spelar- och datorval */
   button.spelarval {
    background-color: greenyellow;
   }
   button.datorval {
    border: rgb(255, 0, 0) solid 2px;
   }

   /* Stil för nollställningsknapp */
   #nolla {
    margin-top: 2em;
    padding: .3em .6em;
    font-size: .8em;
   }

   #nolla:hover {
    background-color: #ffd900ad;
    transition: background-color 0.3s;
   }

   /* Stilar för input-sektion */
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

   /* Stil för huvudrubrik */
   h1 {
    font-size: 39px;
    margin-bottom: 1em;
    color: #ffffff;
    -webkit-text-stroke: 1px rgb(0, 0, 0); 
   }
</style>