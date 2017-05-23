<template>
    <div class="shopcart">
        <div class="content">
            <div class="content-left">
                <span class="logo-wrapper">
                    <div class="logo" :class="{active: selectGoodsArr.length > 0}">
                        <i class="icon-shopping_cart"></i>
                    </div>
                    <div class="num" v-show="selectGoodsArr.length > 0">
                        {{goodNum}}
                    </div>
                </span>
                <span class="price">
                    10
                </span>
                <span class="desc">
                    另需配送费{{deliveryPrice}}元
                </span>
            </div><div class="content-right" :class="priceClass">
                <div class="price">
                    {{priceDesc}}
                </div>
            </div>
        </div>
    </div>
</template>

<script type="text/ecmascript-6">
    export default{
        props: {
            deliveryPrice: {
                type: Number,
                default: 0
            },
            minPrice: {
                type: Number,
                default: 0
            },
            selectGoodsArr: {
                type: Array,
                default: () => {
                    return [
                        {
                            price: 12,
                            count: 3
                        },
                        {
                            price: 12,
                            count: 3
                        },
                        {
                            price: 12,
                            count: 3
                        }
                    ];
                }
            }
        },

        computed: {
            priceDesc: function(){
                var totalPrice = 0;
                this.selectGoodsArr.forEach((item) => {
                    totalPrice += item.price * item.count;
                });

                if(totalPrice === 0){
                    return `${this.minPrice}元起送`;
                }else if(totalPrice < this.minPrice){
                    var diff = this.minPrice - totalPrice;
                    return `还差${diff}元起送`;
                }else if(totalPrice >= this.minPrice){
                    return '去结算';
                }
            },

            priceClass: function(){
                var totalPrice = 0;
                this.selectGoodsArr.forEach((item) => {
                    totalPrice += item.price * item.count;
                });

                if(totalPrice < this.minPrice){
                    return '';
                }else{
                    return 'active';
                }
            },

            goodNum: function(){
                let count = 0;
                this.selectGoodsArr.forEach((item) => {
                    count += item.count;
                });

                return count;
            }
        }
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    .shopcart
        position: fixed
        bottom: 0
        height: 48px
        width: 100%
        .content
            display: flex
            height: 100%
            background-color: #141d27
            .content-left
                flex: 1
                font-size: 0
                .logo-wrapper
                    display: inline-block
                    position: relative
                    top: -10px
                    box-sizing: border-box
                    padding: 6px
                    margin: 0 12px
                    height: 58px
                    width: 58px
                    border-radius: 50%
                    background-color: #141d27
                    .logo
                        height: 100%
                        width: 100%
                        border-radius: 50%
                        background-color: #2b343c
                        text-align: center
                        color: rgba(255, 255, 255, 0.4)
                        .icon-shopping_cart
                            font-size: 24px
                            line-height: 44px
                        &.active
                            background-color: #00a0dc
                            color: #fff
                    .num
                        position: absolute
                        width: 24px
                        height: 16px
                        left: 32px
                        top: 0px
                        font-size: 9px
                        font-weight: 700
                        color: #fff
                        line-height: 16px
                        text-align: center
                        border-radius: 12px
                        background-color: rgb(240, 20, 20)
                        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)
                .price
                    display: inline-block
                    padding-right: 12px
                    margin-top: 12px
                    vertical-align: top
                    font-size: 16px
                    color: rgba(255, 255, 255, 0.4)
                    font-weight: 700
                    line-height: 24px
                    border-right: 1px solid rgba(255, 255, 255, 0.4)
                .desc
                    display: inline-block
                    margin: 12px 0 0 12px
                    vertical-align: top
                    font-size: 16px
                    color: rgba(255, 255, 255, 0.4)
                    font-weight: 700
                    line-height: 24px
            .content-right
                flex: 0 0 105px
                background-color: #2b343c
                text-align: center
                .price
                    width: 100%
                    height: 100%
                    margin-top: 12px
                    font-size: 12px
                    color: rgba(255, 255, 255, 0.4)
                    font-weight: 700
                    line-height: 24px
                &.active
                    background-color: #00b43c
                    .price
                        color: #fff

</style>
