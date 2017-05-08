<template>
    <div class="goods">
        <div class="menu-wrapper" v-el:menu-wrapper>
            <ul>
                <li v-for="item in goods" class="menu-item js_menu" :class="{current: currentIndex === $index}">
                    <span class="text"> <i class="icon" v-if="item.type >= 0" :class="classMap[item.type]"></i>{{item.name}}</span>
                </li>
            </ul>
        </div>
        <div class="foods-wrapper" v-el:foods-wrapper>
            <ul>
                <li v-for="item in goods">
                    <div class="title">
                        {{item.name}}
                    </div>
                    <ul>
                        <li v-for="food in item.foods" class="food-item">
                            <div class="icon">
                                <img :src="food.icon" width="57" height="57" />
                            </div>
                            <div class="content">
                                <div class="name">{{food.name}}</div>
                                <div class="des" v-if="food.description">{{food.description}}</div>
                                <div class="extra">
                                    <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                                </div>
                                <div class="price">
                                    <span class="now">
                                        <span class="huobi">$</span>{{food.price}}
                                    </span>
                                    <span v-if="food.oldPrice" class="old">
                                        <span class="huobi">$</span>{{food.oldPrice}}
                                    </span>
                                </div>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</template>

<script type="text/ecmascript-6">
    import BScroll from 'better-Scroll';

    const errOK = 0;
    export default {
        data(){
            return {
                goods: [],
                scrollY: 0
            };
        },
        created(){
            this.$http.get('/api/goods').then((res) => {
                res = res.body;
                console.log(res);
                if (res.error === errOK){
                    this.goods = res.data;
                    console.log(this.goods);

                    this.$nextTick(() => {
                        this._initScroll();
                    });
                }
            });

            this.classMap = [
                'icon-decrease',
                'icon-discount',
                'icon-special',
                'icon-invoice',
                'icon-guarantee'
            ];
        },
        methods: {
            _initScroll(){
                console.log('this.$els:' + this.$els.menuWrapper);
                this.menuScroll = new BScroll(this.$els.menuWrapper, {});
                this.foodsScroll = new BScroll(this.$els.foodsWrapper, {
                    probeType: 3
                });

                this.foodsScroll.on('scroll', (pos) => {
                    this.scrollY = Math.abs(Math.round(pos.y));
                });

                this.heightList = [0];
                for (let item of this.$els.menuWrapper.getElementsByClassName('js_menu')){
                    this.heightList.push(item.clientHeight);
                }
            }
        },

        computed: {
            currentIndex(){
                for (let i in this.heightList){
                    let height1 = this.heightList[i];
                    let height2 = this.heightList[i + 1];
                    if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)){
                        return i;
                    }
                }
                return 0;
            }
        }

    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    @import '../../common/stylus/mixin.styl'
    .goods
        display: flex
        position: absolute
        width: 100%
        top: 174px
        bottom: 46px
        overflow: hidden
        .menu-wrapper
            flex: 0 0 80px
            width: 80px
            backgroun-color: #f3f5f7
            .menu-item
                display: table
                width: 56px
                height: 54px
                padding: 0 12px
                line-height: 14px
                .icon
                    display: inline-block
                    margin-right: 2px
                    width: 12px
                    height: 12px
                    vertical-align: top
                    background-size: 12px 12px
                    background-repeat: no-repeat
                    &.icon-decrease
                        bg-img("decrease_3")
                    &.icon-discount
                        bg-img("discount_3")
                    &.icon-guarantee
                        bg-img("guarantee_3")
                    &.icon-invoice
                        bg-img("invoice_3")
                    &.icon-special
                        bg-img("special_3")
                .text
                    display: table-cell
                    width: 56px
                    vertical-align: middle
                    font-size: 12px
                    font-weight: 200
                    border-1px(rgba(7, 17, 27, 0.1))
        .foods-wrapper
            flex: 1
            .title
                padding-left: 14px
                height: 26px
                background: #f3f5f7
                font-size: 12px
                color: rgb(147, 153, 159)
                line-height: 26px;
            .food-item
                display: flex
                margin: 18px
                padding-bottom: 18px;
                border-1px(rgba(7, 17, 27, 0.1))
                &:last-child
                    border-none()
                    padding-bottom: 0px
            .icon
                flex: 0 0 57px
                margin-right: 10px
            .content
                flex: 1
                .name
                    margin-top: 2px
                    font-size: 14px
                    line-height: 14px;
                    color: rgb(7, 17, 27)
                .des, .extra
                    margin-top: 8px
                    font-size: 10px
                    line-height: 10px
                    color: rgb(147, 153, 159)
                    .count
                        margin-right: 12px
                .price
                    margin-top: 8px
                    .huobi
                        font-size: 10px
                        font-weight: normal
                    .cur, .old
                        font-weight: 700
                        line-height: 24px
                    .now
                        margin-right: 8px
                        font-size: 14px
                        color: rgb(240, 20, 20)
                    .old
                        text-decoration:line-through;
                        font-size: 10px
                        color: rgb(147, 153, 159)


</style>