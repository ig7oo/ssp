<script setup>
 import { watch } from 'vue';

 const props = defineProps(['knappar', 'reset'])
 const emit = defineEmits(['valdaKnappar'])

 // Funktion som hanterar spelarens val när en knapp klickas
 function spelarval(e) {
  let buttons = document.getElementsByClassName('alternativ');
  // Tar bort tidigare spelarval-markering från alla knappar
  for (let b of buttons) {
    b.classList.remove('spelarval')
  }
  // Markerar den valda knappen som spelarens val
  e.target.classList.add('spelarval')
  // Skickar både spelarens och datorns val till förälder-komponenten
  emit('valdaKnappar', {spelare: e.target.textContent, dator: datorval()})
 }

 // Funktion som slumpar fram datorns val
 function datorval() {
  let val = Math.floor(Math.random() * props.knappar.length);
  let buttons = document.getElementsByClassName('alternativ');
  // Tar bort tidigare datorval-markering och titel från alla knappar
  for (let b of buttons) {
    b.classList.remove('datorval')
    b.title=""
    // Markerar den slumpade knappen som datorns val
    if(b.textContent === props.knappar[val]) {
      b.classList.add('datorval')
      b.title="Datorns val"
    }
  }
  return props.knappar[val]
 }

 // Watch-funktion som övervakar reset-prop för att återställa spelet
 watch(() => props.reset, () => {
  if (props.reset) {
    let buttons = document.getElementsByClassName('alternativ')
    // Tar bort alla markeringar och titlar när spelet återställs
    for (let b of buttons) {
      b.classList.remove('spelarval')
      b.classList.remove('datorval')
      b.title=""
    }
  }
 })

</script>

<template>
    <div class="knapprad">
        <button v-for="knapp in props.knappar" class="alternativ" :key="knapp" @click="spelarval">
        {{ knapp }}
        </button>
    </div>
</template>

<style scoped>
/* Grundstyling för knapparna */
button {
  padding: .6em 1.2em;
  font-size: 1.2em;
  background-color:#ffffffc9;
  border: 1px solid #000000cc;
  border-radius: 5px;
  cursor: pointer;
  }

  /* Hover-effekt för knapparna */
  button:hover {
  background-color: #789af1ab;
  }

/* Styling för spelarens valda knapp */
button.spelarval {
  background-color: rgb(153, 255, 0);
  }

/* Styling för datorns valda knapp */
button.datorval {
  border: red solid 2px;
}

/* Layout för knappraden */
.knapprad {
  display: flex;
  justify-content: center;
  gap: .6em;
  }
</style>