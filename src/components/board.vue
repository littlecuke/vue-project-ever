<template>
    <div class="board" :style="{'width' : this.style.width + 'px','height' : this.style.height + 'px'}">
        <child :ref="'child_' + index" v-for="(item,index) in children" @click-move="clickMove" :isComplete="isComplete" :size="size" :lastIndex="lastIndex" :index="index" :image="image" :imageSize="style" :childData="item" :key="'child_' + index" :childSize="childrenStyle"></child>
    </div>
    <span>当前步数 : {{num}}</span>
</template>

<script>
import child from './child'

export default {
    name: 'board',
    components: {
        child
    },
    data() {
        return {
            num: 0,
            size: [],
            image: '',
            children: [],
            lastIndex: 0,
            style: {
                width: '',
                height: '',
            },
            childrenStyle: {
                width: '',
                height: '',
            },
            isComplete: false,
            options: {},
        }
    },
    mounted() {
        window.onkeyup = event => {
            this.onkeyupEvent(event.keyCode);
        }
    },
    methods: {
        keyBoardMove(x, y) {
            var lastObject = this.$refs['child_' + this.lastIndex];
            var _x = lastObject.position.x + x;
            var _y = lastObject.position.y + y;
            for (let i = 0; i < this.children.length; i++) {
                const child = this.$refs['child_' + i];
                if (child.position.x == _x && child.position.y == _y) {
                    this.clickMove(child);
                    break;
                }
            }
        },
        onkeyupEvent(keyCode) {
            if (!this.isComplete && this.children.length) {
                switch (keyCode) {
                    case 38:
                        this.keyBoardMove(1, 0)
                        break;
                    case 40:
                        this.keyBoardMove(-1, 0)
                        break;
                    case 37:
                        this.keyBoardMove(0, 1)
                        break;
                    case 39:
                        this.keyBoardMove(0, -1)
                        break;
                    default:
                        break;
                }
            }
        },
        checkComplete() {
            var isComplete = true;
            for (let i = 0; i < this.children.length; i++) {
                const child = this.$refs['child_' + i];
                if (child.position.x != child.childData.y || child.position.y != child.childData.x) {
                    isComplete = false;
                    break;
                }
            }
            if (isComplete) {
                this.isComplete = isComplete;
                setTimeout(() => {
                    alert('成功');
                }, 200)
            }
        },
        clickMove(clickChild) {
            if (!this.isComplete) {
                var lastObject = this.$refs['child_' + this.lastIndex];
                var last_x = lastObject.position.x;
                var last_y = lastObject.position.y;
                var lastMove = lastObject.changePosition(clickChild.position.x, clickChild.position.y);
                var childMove = clickChild.changePosition(last_x, last_y);
                if (lastMove && childMove) {
                     this.num++;
                }
                this.checkComplete();
            }
        },
        shuffle(arr) {
            for (var i = arr.length - 1; i >= 0; i--) {
                var randomIndex = Math.floor(Math.random() * (i + 1));
                var itemAtIndex = arr[randomIndex];
                arr[randomIndex] = arr[i];
                arr[i] = itemAtIndex;
            }
            return arr;
        },
        setDataEvent(data) {
            this.num = 0;
            this.style.width = data.width;
            this.style.height = data.height;
            this.childrenStyle.width = data.width / data.x;
            this.childrenStyle.height = data.height / data.y;
            this.children = [];
            this.image = data.image;
            this.size = [data.x, data.y];
            this.isComplete = false;
            this.$nextTick(() => {
                var _children = [];
                for (let y = 0; y < data.y; y++) {
                    for (let x = 0; x < data.x; x++) {
                        var child = {};
                        child.y = y;
                        child.x = x;
                        if (x + 1 == data.x && y + 1 == data.y) {
                            child.last = true;
                        }
                        _children.push(child);
                    }
                }
                this.children = this.shuffle(_children);
                for (let i = 0; i < _children.length; i++) {
                    const child = _children[i];
                    if (child.last) {
                        this.lastIndex = i;
                    }
                }
            });
        },
    },
}
</script>

<style>
    .board { background: #aaa; position: relative;}
</style>