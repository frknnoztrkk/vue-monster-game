<template>
    <div class="hover-overlay-container">
        <img class="overlay-image" src="../assets/image/lobi1.png"/>
        <div class="overlay-btn-container" >
            <button class="overlay-btn" @click="basla()">OYUNA BAŞLA</button>
        </div>
    </div>

</template>

<script>

export default {
    props: ['save'],
    data() {
        return {
            data: {

            }
        }
    },
    methods: {
        basla() {
            this.$props.save(false)
        },
    }
}
</script>

<style>
.hover-overlay-container .overlay-image {
    width: 100%;
    height: 100cqmax;
    margin-bottom: -15%;
}

.hover-overlay-container .overlay-btn-container .overlay-btn {
    background-color: black;
    color: aliceblue;
    font-family: fantasy;
    font-size: larger;
    border-radius: 5px;
    width: 100%;
    padding: 30px;
    margin-top: -50px;
}

@media screen and (min-width:700px) {
    .hover-overlay-container {
        position: relative;
        width: 100%;
        height: 100vh;
    }

    .hover-overlay-container:hover .overlay-image {
        opacity: 0.8;
    }

    .hover-overlay-container:hover .overlay-btn-container {
        opacity: 1;
    }

    .hover-overlay-container .overlay-image {
        display: block;
        width: 100%;
        height: 100vh;
        opacity: 1;
        transition: .5s ease;
        backface-visibility: hidden;
    }

    .hover-overlay-container .overlay-btn-container {
        position: absolute;
        top: 50%;
        left: 50%;
        opacity: 0;
        transition: .5s ease;
        text-align: center;
        transform: translate(-50%, -50%);
    }

    .hover-overlay-container .overlay-btn-container .overlay-btn {
        background-color: black;
        width: 200%;
        height: 150px;
        border-radius: 20px;
        color: aqua;
        text-decoration: none;
        font-size: xx-large;
        color: green;
    }

    .hover-overlay-container .overlay-btn-container .overlay-btn:hover {
        cursor: pointer;
    }
}
</style>