<template>
    <div class="header">
        <div class="content-wrapper">
            <div class="avatar">
                <img :src="seller.avatar" width="64" height="64">
            </div>
            <div class="content">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">
                    {{seller.description}} / {{seller.deliveryTime}}分钟送达
                </div>
                <div v-if="seller.supports" class="support">
                    <span class="icon" :class="classMap[seller.supports[0].type]"></span>
                    <span class="text">{{seller.supports[0].description}}</span>
                </div>
            </div>
            <div v-if="seller.supports" class="supports-count" @click="showDetail">
                <span class="count">
                    {{seller.supports.length}}个
                </span>
                <i class="icon-keyboard_arrow_right"></i>
            </div>
        </div>
        <div class="bulletin-wrapper" @click="showDetail">
            <span class="bulletin-title"></span><span class="bulletin-text">
            {{seller.bulletin}}
        </span>
            <i class="icon-keyboard_arrow_right"></i>
        </div>
        <div class="background">
            <img width="100%" height="100%" :src="seller.avatar">
        </div>
        <div class="detail" v-show="detailShow" transition="fade">
            <div class="detail-wrapper">
                <div class="detail-main">
                    <h1 class="name">{{seller.name}}</h1>
                    <div class="star-wrapper">
                        <star :size="48" :score="seller.score"></star>
                    </div>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">优惠信息</div>
                        <div class="line"></div>
                    </div>
                    <ul class="supports" v-if="seller.supports">
                        <li class="support-item" v-for="item in seller.supports">
                            <span class="icon" :class="classMap[seller.supports[$index].type]"></span>
                            <span class="text">{{item.description}}</span>
                        </li>
                    </ul>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">商家公告</div>
                        <div class="line"></div>
                    </div>
                    <p class="bulletin">
                        {{seller.bulletin}}
                    </p>
                </div>
            </div>
            <div class="detail-close" @click="closeDetail">
                <i class="icon-close"></i>
            </div>

        </div>
    </div>
</template>

<script type="text/ecmascript-6">
    import star from 'components/star/star';

    export default {
        components: {
            'star': star
        },

        props: {
            seller: {
                type: Object
            }
        },

        data: function(){
            return {
                detailShow: false
            };
        },

        created(){
            this.classMap = [
                'icon-decrease',
                'icon-discount',
                'icon-special',
                'icon-invoice',
                'icon-guarantee'
            ];
        },

        methods: {
            showDetail(){
                this.detailShow = true;
            },

            closeDetail(){
                console.log('closeDetail');
                this.detailShow = false;
            }
        }
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    @import '../../common/stylus/mixin.styl'

    .header
        position: relative
        background-color: rgba(7, 17, 27, 0.5)
        .content-wrapper
            position: relative
            padding: 24px 12px 18px 24px
            color: #fff
            font-size: 0
            .avatar
                display: inline-block
                margin-right: 16px
                vertical-align: top
                img
                    border-radius: 2px
            .content
                display: inline-block
                /*font-size: 14px*/
                .title
                    /*font-size: 0px*/
                    margin: 2px 0 8px 0
                    .brand
                        display: inline-block
                        width: 30px
                        height: 18px
                        margin-right: 6px
                        vertical-align: top
                        bg-img("brand")
                        background-size: 30px 18px
                        background-repeat: no-repeat
                    .name
                        font-size: 16px
                        font-weight: bold
                        line-height: 18px

                .description
                    margin-top: 8px
                    font-size: 12px
                    line-height: 12px
                .support
                    margin-top: 10px
                    .icon
                        display: inline-block
                        margin-right: 4px
                        width: 12px
                        height: 12px
                        vertical-align: top
                        background-size: 12px 12px
                        background-repeat: no-repeat
                        &.icon-decrease
                            bg-img("decrease_1")
                        &.icon-discount
                            bg-img("discount_1")
                        &.icon-guarantee
                            bg-img("guarantee_1")
                        &.icon-invoice
                            bg-img("invoice_1")
                        &.icon-special
                            bg-img("special_1")
                    .text
                        font-size: 10px
                        line-height: 12px

            .supports-count
                position: absolute
                right: 12px
                bottom: 14px
                padding: 0 8px
                height: 24px
                line-height: 24px
                border-radius: 14px;
                background-color: rgba(0, 0, 0, 0.2)
                text-align: center
                .count
                    font-size: 10px
                    font-weight: 12px
                    vertical-align: top
                .icon-keyboard_arrow_right
                    margin-left: 2px
                    font-size: 10px
                    line-height: 24px
        .bulletin-wrapper
            position: relative
            height: 28px
            padding: 0 22px 0 12px
            line-height: 28px
            white-space: nowrap
            overflow: hidden
            text-overflow: ellipsis
            color: #fff
            background-color: rgba(7, 17, 27, 0.2)
            .bulletin-title
                display: inline-block
                width: 22px
                height: 12px
                /*vertical-align: top*/
                bg-img("bulletin")
                background-size: 22px 12px
                background-repeat: no-repeat
            .bulletin-text
                margin: 0 4px
                font-size: 10px
                vertical-align: top
            .icon-keyboard_arrow_right
                position: absolute
                font-size: 10px
                right: 12px
                top: 8px
        .background
            position: absolute
            overflow: hidden
            left: 0
            top: 0
            z-index: -1
            width: 100%
            height: 100%
            filter: blur(10px)
        .detail
            position: fixed
            z-index: 100
            overflow: auto
            top: 0px
            left: 0px
            width: 100%
            height: 100%
            transition: all 0.5s
            &.fade-transition
                opacity: 1
                background-color: rgba(7, 17, 27, 0.8)
            &.fade-enter,&.fade-leave
                opacity: 0
              //  background-color: rgba(7, 17, 27, 0)
            .name
                text-align: center
                line-height: 16px
                font-size: 16px
                font-weight: 700
            .detail-wrapper
                /*position: relative*/
                min-height: 100%
                .detail-main
                    padding: 64px 0px 64px 0px
                    color: #fff
                    .star-wrapper
                        height: 24px
                        padding: 16px 0 28px 0
                        text-align: center
                    .title
                        display: flex
                        width: 80%
                        margin: 28px auto 24px auto
                        .line
                            flex: 1
                            position: relative
                            top: -6px
                            border-bottom: 1px solid rgba(255, 255, 255, 0.2)
                        .text
                            margin: 0 12px
                            font-size: 14px
                            font-weight: 700
                    .supports
                        width: 80%
                        padding: 0 12px
                        margin: auto
                        .support-item
                            margin-bottom: 12px
                            &:last-child
                                margin-bottom: 0px
                            .icon
                                display: inline-block
                                margin-right: 6px
                                width: 16px
                                height: 16px
                                vertical-align: top
                                background-size: 16px 16px
                                background-repeat: no-repeat
                                &.icon-decrease
                                    bg-img("decrease_1")
                                &.icon-discount
                                    bg-img("discount_1")
                                &.icon-guarantee
                                    bg-img("guarantee_1")
                                &.icon-invoice
                                    bg-img("invoice_1")
                                &.icon-special
                                    bg-img("special_1")
                            .text
                                font-size: 12px
                                font-weight:200
                                line-height: 16px

                    .bulletin
                        width: 80%
                        padding: 0 12px
                        margin: auto
                        font-size: 12px
                        font-weight: 200
                        line-height: 24px
            /*text-align: center*/
                /*.closetest*/
                    /*position: absolute*/
                    /*bottom: 64px*/
                    /*text-align: center*/
                    /*font-size: 32px*/
            .detail-close
                position: relative
                width: 32px
                height: 32px
                margin: -64px auto 0 auto
                font-size: 32px

</style>
