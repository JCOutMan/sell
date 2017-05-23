<template>
    <div class="cartControl">
        <div class="sub icon-remove_circle_outline"></div>
        <div class="num">1</div>
        <div class="add icon-add_circle" @click="addCount"></div>
    </div>
</template>

<script type="text/ecmascript-6">
    export default {
        props: {
            cartGood: {
                type: Object
            }
        },

        methods: {
            addCount(){

            }
        }
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    .cartControl
        font-size: 0
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
