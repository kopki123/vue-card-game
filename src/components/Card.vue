<template>
  <div class="card" @click="filp" :class="{ visible: card.visible, checked: card.checked}">
      <div class="card-front">
           <img src="" alt="">
            <h1 v-if="card.checked">{{ card.num }}</h1>  
      </div>
      <div class="card-back"></div>
  </div>
</template>

<script>

export default {
    props: {
        card: Object,
        matchedCardArray: Array
    },
    setup(props, context) {
        const filp = () => {
            context.emit('filpcard', props.card.id);
        }

        return { filp };
    }
}
</script>

<style scoped>
.card {
    width: 120px;
    height: 150px;
    border: 1px solid rgb(255, 208, 208);
    background: rgb(255, 208, 208);
    border-radius: 10px;
    cursor: pointer;
    margin-left: 30px;

    position: relative;
    transition: 0.75s ease-in-out;
    transform-style: preserve-3d;
}

.checked {
    /* background: rgb(255, 255, 255); */
    transform: rotateY(180deg);
}

.card-front {
    transform: rotateY(180deg) translateZ(1px);
}

.card-front {
    background: rgb(255, 255, 255);
}

.card-back {
    /* background: yellowgreen; */
}

.card-front,
.card-back {
    width: 100%;
    height: 100%;
    border-radius: 10px;


    position: absolute;
    top: 0;
    left: 0;
    overflow: hidden;

}

.checked .h1 {
    transform: rotateY(180deg);
}

.visible {
    pointer-events: none;
}

h1 {
    color: black;
}

</style>