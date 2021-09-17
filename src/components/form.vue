<template>
    <div class="boardForm">
        <label for="width">宽度 : </label>
        <input type="text" name="width" v-model="style.width">
        <span> 像素</span>
        <br><br>
        <label for="height">高度 : </label>
        <input type="text" name="height" v-model="style.height">
        <span> 像素</span>
        <br><br>
        <span>尺寸 : </span>
        <label for="X">x : </label>
        <input type="text" name="X" class="smallInput" v-model="size.x">
        &nbsp;&nbsp;
        <label for="Y">y : </label>
        <input type="text" name="Y" class="smallInput" v-model="size.y">
        <br><br>
        <label for="image">选择图片 : </label>
        <input type="file" @input="imageChange($event)" name="image">
        <br><br>
        <label for="image">预览 : </label>
        <div class="preview">
            <img class="previewImage" :src="image" alt="">
        </div>
        <br><br>
        <button class="button" @click="submit">确定</button>
    </div>
</template>

<script>
// import state from '../state'


export default {
    name: 'boardForm',
    data() {
        return {
            style: {
                width: 300,
                height: 300,
            },
            size: {
                x: 3,
                y: 3
            },
            fileData: '',
            image: '',
        }
    },
    methods: {
        drawing(file) {
            var canvas = document.createElement('canvas');
            canvas.width = this.style.width;
            canvas.height = this.style.height;
            var ctx = canvas.getContext('2d');
            var img = new Image();
            img.setAttribute('crossOrigin', 'anonymous');
            img.onload = () => {
                ctx.drawImage(img, 0, 0, this.style.width, this.style.height);
                canvas.toBlob(blobObj => {
                    this.image = window.webkitURL.createObjectURL(blobObj)
                })
            }
            img.src = window.webkitURL.createObjectURL(file);
        },
        imageChange(event) {
            window.webkitURL.revokeObjectURL(this.image)
            this.drawing(event.target.files[0]);
            event.target.value = '';
        },
        submit() {
            this.$emit('change-value', {
                ...this.style,
                image: this.image,
                ...this.size,
            })
        },
    },
}
</script>

<style>
    .boardForm { position: absolute; left: 50%; top: 0; padding: 20px;}
    .smallInput { width: 40px; }
    .preview { width: 100px; height: 100px; background: #ccc; display: inline-block; vertical-align: top;}
    .previewImage { display: block; width: 100%; height: 100%;}
</style>