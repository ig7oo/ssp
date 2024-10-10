<script setup>
import { ref, watch } from 'vue';

const props = defineProps(['vinnare']);
const score = ref({ spelare: 0, dator: 0 });
watch(props, () => {
    if (props.vinnare == 'spelare') {
        score.value.spelare++
    } else if(props.vinnare==='dator'){ 
        score.value.dator++
    }
})
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