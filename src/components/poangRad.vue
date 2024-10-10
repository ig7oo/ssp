<script setup>
import { ref, watch } from 'vue';


const props = defineProps(['knappar', 'reset', 'vinnare'])
const score = ref({ spelare: 0, dator: 0 });
watch(props, () => {
    if (props.vinnare == 'spelare') {
        score.value.spelare++
    } else if(props.vinnare==='dator'){ 
        score.value.dator++
    }
})

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
    <div class="score">
        <p>
            <span id="spelare">
                {{ score.spelare }}
            </span> - <span id="dator">
                {{ score.dator }}
            </span>
        </p>
    </div>
</template>

<style scoped>
</style>