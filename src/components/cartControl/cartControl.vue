<template>
    <div class="cartControl">
        <div class="sub icon-remove_circle_outline" v-show="food.count > 0" @click="subCount" transition="move"></div>
        <div class="num" v-show="food.count > 0">{{food.count}}</div>
        <div class="add icon-add_circle" @click="addCount"></div>
    </div>
</template>

<script type="text/ecmascript-6">
    import Vue from 'vue';
    export default {
        data(){
            return {
            };
        },

        props: {
            food: {
                type: Object
            }
        },

        methods: {
            addCount(event){
                console.log('add');
                if(!this.food.count){
                    Vue.set(this.food, 'count', 1);
                }else{
                    this.food.count++;
                }
                this.$dispatch('cart.add', event.target);
            },

            subCount(){
                console.log('sub');
                this.food.count > 0 && this.food.count--;
            }
        }
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    .cartControl
        font-size: 0
        .sub
            display: inline-block
            transition: all 0.4s linear
            &.move-transition
                opacity: 1
                transform: translate3D(0, 0, 0) rotate(0)
            &.move-enter,&.move-leave
                opacity: 0
                transform: translate3D(24px, 0, 0) rotate(180deg)
        .sub,.add
            display: inline-block
            font-size: 24px
            line-height: 24px
            color: rgb(0, 160, 220)
        .num
            display: inline-block
            width: 24px
            text-align: center;
            vertical-align: top;
            font-size: 10px
            line-height: 24px
            color: rgb(147, 153, 159)

</style>
