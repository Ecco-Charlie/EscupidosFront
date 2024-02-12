<template>
    <div class="card">
        <MessageCredentials :parametro="mensaje?.remitente"/>
        <h3>{{ mensaje?.mensaje }}</h3>
        <div class="deco">
            <h3>10k</h3>
            <h3>20k</h3>
            <h3>1k</h3>
        </div>
    </div>
</template>

<script lang="ts" setup>
     
    import { ref, onMounted } from 'vue';
    import MessageCredentials from './MessageCredentials.vue';

    interface Mensaje {
    id: number;
    tipo: string;
    mensaje: string;
    remitente: string;
    destino: string;
    imagen: string;
    }

    const mensaje = ref<Mensaje | null>(null);
    const imagenUrl = ref<string>('');

    const fetchMessage = async () => {
    try {
        const response = await fetch(`http://soft-exe.ddns.net:8080/msg`);
        const data = await response.json();

        mensaje.value = {
        id: data.id,
        tipo: data.tipo,
        mensaje: data.mensaje,
        remitente: data.remitente,
        destino: data.destino,
        imagen: data.imagen
        };


        imagenUrl.value = `data:image/jpeg;base64,${data.imagen}`;
    } catch (error) {
        console.error(error);
        throw new Error('Error al obtener el mensaje');
    }
    };

    onMounted(fetchMessage);
</script>

<style scoped>
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
</style>