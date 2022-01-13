<script>

export default {
    props: {
        matched: {
            type: Boolean,
            default: false
        },
        position : {
            type: Number,
            required:true
        },
        value: {
            type: String,
            required: true
        },
        visible: {
            type: Boolean,
            default: false
        },
    },
    setup(props, context){
        const selectCard = () => {
            context.emit("select-card", {
                position:props.position,
                faceCardValue: props.value
            })
        }
        return {
            selectCard
        }
    }
}
</script>

<template>
    <div class="card" @click="selectCard">
        <div v-if="visible" class="card-face">
            <img :src="`images/${value}.png`" /> 
        </div>
        <div v-else class="card-back">
        </div> 
    </div>
</template>

<style>
.card {
  position: relative;
}
.card >.card-face, .card-back {
    width: 100px;
    height: 100px;
    position: absolute;
}
.card-face {
    color: white;
    border-radius: 10px;
    opacity: 50%;
}
.card-back {
    border-radius: 10px;
    background-image: url('/images/back.png');
    color: white;
}
.card-back:hover{
    opacity: 50%;
}
</style>