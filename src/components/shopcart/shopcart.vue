<template>
    <div class="shopcart">
        <div class="content">
            <div class="content-left">
                <span class="logo-wrapper">
                    <div class="logo">
                        <i class="icon-shopping_cart"></i>
                    </div>
                </span>
                <span class="price">
                    10
                </span>
                <span class="desc">
                    另需配送费{{deliveryPrice}}元
                </span>
            </div><div class="content-right">
                <div class="price">
                    {{minPrice}}元起送
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
            .content-left
                flex: 1
                background-color: #141d27
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
                        .icon-shopping_cart
                            color: rgba(255, 255, 255, 0.4)
                            font-size: 24px
                            line-height: 44px
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
                    margin-top: 12px
                    font-size: 12px
                    color: rgba(255, 255, 255, 0.4)
                    font-weight: 700
                    line-height: 24px
</style>
