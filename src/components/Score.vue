<template>
    <div class="backdrop">
        <div class="modal">
            <h1>{{title}}</h1>
            <p>Gra skończona w {{moves}} ruchach</p>
            <button @click="$emit('again')">Zagraj ponownie</button>
        </div>
    </div>
</template>

<script>

export default {
    props: ['title', 'moves'],
}

</script>

<style>
.modal{
    width: 400px;
    padding: 20px;
    margin: 100px auto;
    background:white;
    border-radius: 10px;
}
.backdrop{
    top: 0;
    position: fixed;
    background-color: rgba(0, 0, 0, 0.5);
    width: 100%;
    height: 100%;
}
h1{
    color: #03cfb4;
}
button{
    background-color: #03cfb4;
    color:white;
    border-radius: 10px;
    border: 1px solid #03b9a1;
    width:100px;
    height:50px;
}
</style>