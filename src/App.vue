<script setup>
 import { ref } from 'vue';
 import knappRad from './components/knappRad.vue';

 const score = ref ({ spelare: 0, dator: 0});
 const resultat = ref('Välj sten, sax eller påse!');
 const knappar = ref (['Sten', 'Sax', 'Påse', 'Lizard', 'Spock']);





function hittaVinnare() {
    let buttons = document.getElementsByClassName('alternativ')
    for (let b of buttons) {
        if (b.classList.contains('spelarval')) {
            var spelarval = b.textContent
        }
        if (b.classList.contains('datorval')) {
            var datorval = b.textContent
        }
    }

    if (spelarval == datorval) {
        resultat.value = 'Oavgjort!'
    } else if (
        spelarval == 'Sten' && datorval == 'Sax'|| 
        spelarval == 'Sax' && datorval == 'Påse'|| 
        spelarval == 'Påse' && datorval == 'Sten'
      ) {
        resultat.value = 'Du vann!'
        score.value.spelare++
    } else {
        resultat.value = 'Du förlorade!'
        score.value.dator++
    }
}
function reset() {
    score.value.spelare = 0
    score.value.dator = 0
    resultat.value = 'Välj sten, sax eller påse!'
    let buttons = document.getElementsByClassName('alternativ')
    for (let b of buttons) {
        b.classList.remove('spelarval')
        b.classList.remove('datorval')
    }
}



</script>

<template>
  <header>
    <h1>Sten, sax, påse!</h1>
  </header>

  <main>
    <knappRad :knappar="knappar" @valda-knappar="hittaVinnare"/>
    <div class="resultat">
      <p id="resultat">{{resultat}}</p>
    </div>
    <div class="score">
      <p>
        <span id="spelare">{{ score.spelare }}</span> -
        <span id="dator">{{ score.dator }}</span>
      </p>
      <button id="nolla" @click="reset">Nollställ poängen</button>
      
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
</style>
