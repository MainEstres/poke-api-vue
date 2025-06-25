<script setup>
import { ref } from 'vue';

const emit = defineEmits(['send']);

const props = defineProps({
    name: String,
    image: String,
    descubierto: Boolean
})

const inputPoke = ref('');

const send = (e)=> {
    e.preventDefault()
 if(inputPoke.value.toLowerCase().trim() === props.name.toLowerCase()){
    props.descubierto = true
    emit('send', props.name)
 }else{
    alert('Sigue intentandooooo...')
 }
}

</script>


<template>

    <div class="card border-0 my-3" style="width: 16rem;">
        <img :class="[!descubierto ? 'filter' : '', 'pokemon-image', 'mx-auto']" :src=image>
        <span v-if="descubierto">{{ name }}</span>
        <form class="mt-2" v-show="!descubierto" >
            <input placeholder="Nombre Pokemon" v-model="inputPoke" type="text">
            <button type="submit" @click="send" class="btn btn-outline-success my-2">Enviar</button>
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