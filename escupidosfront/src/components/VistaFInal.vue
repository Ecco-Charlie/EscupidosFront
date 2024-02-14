<template>
    <div class="card" v-show="mensaje?.tipo=='TWITTER'" id="container">
        <div class="credenciales">
            <img src="" alt="icono">
            <h1>{{ mensaje?.remitente }}</h1>
        </div>
        <h2>@{{ mensaje?.destino }}</h2>
        <h3>{{ mensaje?.mensaje }}</h3>
        <div class="deco">
            <h3>10k</h3>
            <h3>20k</h3>
            <h3>1k</h3>
        </div>
    </div>
    
    
    <div class="card-insta" v-show="mensaje?.tipo=='INSTAGRAM'">
        <h3>Te amo uwu</h3>
        <img :src="imageUrl" alt="">    
        <div class="iconos">
            <img src="" alt="Me encanta">
            <img src="" alt="Comentar">
            <img src="" alt="Enviar">
        </div>
    </div>
</template>

<script lang="ts" setup>

import { ref, onMounted, onBeforeUnmount, ErrorCodes } from 'vue';

let insta = false;
interface Mensaje {
id: number;
tipo: string;
mensaje: string;
remitente: string;
destino: string;
imagen: Uint32Array;
}

let imageUrl:string;
const mensaje = ref<Mensaje | null>(null);

//Funcion para pasar a formato base 64 

const fetchMessage = async () => {
try {
    const response = await fetch(`http://soft-exe.ddns.net:8080/escupidos/msg`);
    const data = await response.json();
    console.log(data);
    mensaje.value = {
    id: data.id,
    tipo: data.tipo,
    mensaje: data.mensaje,
    remitente: data.remitente,
    destino: data.destino,
    imagen: data.imagen
    };
    imageUrl = `data:image/jpeg;base64,${data.imagen}`
    insta = (data.tipo === "INSTAGRAM");
    console.log(insta);

} catch (error) {
    console.error(error);
    console.log(ErrorCodes)
    throw new Error('Error al obtener el mensaje');
}

};


const interval = setInterval(fetchMessage, 3000); // Temporizador para llamar a fetchMessage cada 10 segundos

onMounted(fetchMessage);

onBeforeUnmount(() => {
clearInterval(interval); // Limpiar el temporizador cuando el componente se desmonte para evitar fugas de memoria
});
</script>

<style scoped>
    h2{
        padding-left:5rem;
        padding-right:5rem;
    }

    h3{
        padding-left: 5rem;
        padding-right: 5rem;
        font-size: 34px;
    }
    .card{
        width: 80vw;
        height: auto;
        background-color: red;
        border-radius: 35px;
        margin-left:7rem ;
    }
    .deco{
        display: flex;
        flex-direction: row;
    }

    .credenciales{
        display: flex;
        flex-direction: row;
        align-items: center;
        padding: 40px;
    }

    .card-insta{
        width: 60vw;
        height: auto;
        background-color: blue;
        border-radius: 35px;
        margin-left:12rem ;
        
    }

</style>