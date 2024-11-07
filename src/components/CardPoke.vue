<script setup>
import { ref } from 'vue';

const emit = defineEmits(['send']);

const props = defineProps({
    name: String,
    image: String,
    descubierto: Boolean
})

const inputPoke = ref('');

const send = ()=> {
 if(inputPoke.value.toLowerCase().trim() === props.name.toLowerCase()){
    props.descubierto = true
    emit('send', props.name)
 }else{
    alert('Sigue intentandooooo...')
 }
}

</script>


<template>

    <div class="card border border-0 my-3" style="width: 16rem;">
        <img :class="[!descubierto ? 'filter' : '', 'pokemon-image', 'mx-auto']" :src=image>
        <span v-if="descubierto">{{ name }}</span>
        <form v-show="!descubierto" @submit.prevent="send">
            <input v-model="inputPoke" type="text">
            <button>Enviar</button>
        </form>
    </div>


</template>

<style scoped>
.pokemon-image {
    width: 10rem;
    height: 10rem;
}

.filter {
    filter: blur(5px) grayscale(100%);
}
</style>