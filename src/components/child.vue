<template>
    <div class="child" :style="{
        'width' : childSize.width + 'px', 
        'height' : childSize.height + 'px',
        'top' : position.x * childSize.height + 'px',
        'left' : position.y * childSize.width + 'px',
        }" @click="clickMove">
        <img class="childImage" v-show="!childData.last || isComplete" :style="{
        'width' : imageSize.width + 'px', 
        'height' : imageSize.height + 'px',
        'transform': 'translateX(-' + (childData.x * childSize.width)  + 'px) translateY(-'+  (childData.y * childSize.height) +'px)'
        }" :src="image" alt="">
    </div>
</template>

<script>
export default {
    props: ['image', 'childData', 'imageSize', 'childSize', 'size', 'index', 'lastIndex', 'isComplete'],
    data() {
        return {
            position: {
                x: parseInt(this.index / this.size[0]),
                y: parseInt(this.index - (parseInt(this.index / this.size[0]) * this.size[0])),
            },
        }
    },
    mounted(){
        console.log(this.image);
    },
    methods: {
        clickMove() {
            this.$emit('clickMove', this)
        },
        changePosition(x, y) {
            if (x == this.position.x && Math.abs(this.position.y - y) == 1) {
                this.position.y = y;
                return true;
            } else if (y == this.position.y && Math.abs(this.position.x - x) == 1) {
                this.position.x = x;
                return true
            } else {
                return false
            }
        },
    },
}
</script>

<style>
    .child { transition: all 0.2s; overflow: hidden; position: absolute;}
    .childImage { display: block; position: absolute; top: 0; left: 0;}
</style>