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
            type: Number,
            required: true
        },
        visible: {
            type: Boolean,
            default: false
        }
    },



    setup(props, context){
        console.log("props",props.value)
        // console.log("context",context)
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
            {{value}} - {{matched}}
        </div>
        <div v-else class="card-back">
            Back
        </div>
        
    </div>
</template>

<style>
.card {
  border: 5px solid #ccc;
  position: relative;
}

.card >.card-face, .card-back {
    width: 100px;
    height: 100px;
    position: absolute;

}

.card-face {
    background-color: red;
    color: white;
    
}

.card-back {
    background-color: blue;
    color: white;
}
</style>