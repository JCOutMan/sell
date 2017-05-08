<template>
<div class="star" :class="starType">
    <span v-for="itemClass in itemClassed" :class="itemClass" class="star-item" track-by="$index"></span>
</div>
</template>

<script type="text/ecmascript-6">
    const LENGTH = 5;
    const CLS_ON = 'on';
    const CLS_HALF = 'half';
    const CLS_OFF = 'off';

    export default{
        props: {
            size: {
                type: Number
            },
            score: {
                type: Number
            }
        },

        computed: {
            starType(){
                return 'star-' + this.size;
            },

            itemClassed(){
                let score = Math.floor(this.score * 2) / 2;
                var isFloat = (score % 1 !== 0);
                var arr = [];
                for (var i = 0; i < Math.floor(score); i++){
                    arr.push(CLS_ON);
                }

                if (isFloat){
                    arr.push(CLS_HALF);
                };

                while (arr.length < LENGTH){
                    arr.push(CLS_OFF);
                };

                return arr;
            }
        }
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    @import '../../common/stylus/mixin.styl';
    .star
        display: inline-block
        .star-item
            display: inline-block
            background-repeat: no-repeat
        &.star-48
            .star-item
                margin-right: 22px
                width: 20px
                height: 20px
                background-size: 20px 20px
                &:last-child
                    margin-right: 0px
                &.on
                    bg-img("star48_on")
                &.off
                    bg-img("star48_off")
                &.half
                    bg-img("star48_half")
        &.star-36
            .star-item
                margin-right: 6px
                width: 15px
                height: 15px
                background-size: 15px 15px
                &:last-child
                    margin-right: 0px
                &.on
                    bg-img("star36_on")
                &.off
                    bg-img("star36_off")
                &.half
                    bg-img("star36_half")
        &.star-24
            .star-item
                margin-right: 3px
                width: 10px
                height: 10px
                background-size: 10px 10px
                &:last-child
                    margin-right: 0px
                &.on
                    bg-img("star24_on")
                &.off
                    bg-img("star24_off")
                &.half
                    bg-img("star24_half")
</style>
