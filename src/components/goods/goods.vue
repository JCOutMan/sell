<template>
    <div class="goods">
        <div class="menu-wrapper" v-el:menu-wrapper>
            <ul>
                <li v-for="item in goods" class="menu-item" @click="clickMenu($index, $event)" :class="{'current': currentIndex == $index}">{{currentIndex}}:{{$index}}
                    <span class="text"> <i class="icon" v-if="item.type >= 0" :class="classMap[item.type]"></i>{{item.name}}</span>
                </li>
            </ul>
        </div>
        <div class="foods-wrapper" v-el:foods-wrapper>
            <ul>
                <li v-for="item in goods" class="js_goodItem">
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
                            <div class="cartControlWrapper">
                                <cart-control :food="food"></cart-control>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <v-shopcart v-ref:shopcart :select-goods-arr="selectGoodsArr" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></v-shopcart>
    </div>
</template>

<script type="text/ecmascript-6">
    import BScroll from 'better-Scroll';
    import shopcart from '../shopcart/shopcart.vue';
    import cartControl from '../cartControl/cartControl.vue';

    const errOK = 0;
    export default {
        props: {
            seller: {
                type: Object,
                default: {}
            }
        },
        components: {
            'v-shopcart': shopcart,
            'cartControl': cartControl
        },

        data(){
            return {
                goods: [],
                scrollY: 0,
                heightList: [0]
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
                        this._calculateHeight();
                    });
                }
            });

            this.classMap = [
                'icon-dedncrease',
                'icon-discount',
                'icon-special',
                'icon-invoice',
                'icon-guarantee'
            ];
        },
        methods: {
            clickMenu(index, event){
                console.log(index);
                if (!event._constructed){
                    return;
                }
                let foodsWrapper = this.$els.foodsWrapper.getElementsByClassName('js_goodItem');
                let el = foodsWrapper[index];
                this.foodsScroll.scrollToElement(el);
            },

            _drop(target){
                this.$refs.shopcart.drop(target);
            },

            _initScroll(){
                console.log('this.$els:' + this.$els.menuWrapper);
                this.menuScroll = new BScroll(this.$els.menuWrapper, {
                    click: true
                });
                this.foodsScroll = new BScroll(this.$els.foodsWrapper, {
                    probeType: 3
                });

                this.foodsScroll.on('scroll', (pos) => {
                    this.scrollY = Math.abs(Math.round(pos.y));
                    console.log('this.scrollY:', this.scrollY);
//                    console.log(this);
                });
            },

            _calculateHeight(){
                this.heightList = [0];
                let menuItems = this.$els.foodsWrapper.getElementsByClassName('js_goodItem');
                console.log('menuItems length:' + menuItems.length);
                for (let item of menuItems){
                    console.log('item.clientHeight', item.clientHeight);
                    this.heightList.push(item.clientHeight);
                }
            }
        },

        computed: {
            currentIndex(){
                console.log('currentIndex heightList:', this.heightList);
                for (let i in this.heightList){
                    let height1 = this.heightList[i];
                    let height2 = this.heightList[i + 1];
                    console.log('height1:' + height1 + 'height2:' + height2);
                    if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)){
                        return i;
                    }
                }
                return 0;
            },

            selectGoodsArr(){
                var arr = [];
                this.goods.forEach((good) => {
                    good.foods.forEach(food => {
                        if(food.count){
                            arr.push(food);
                        }
                    });
                });
                return arr;
            }
        },

        events: {
            'cart.add'(target){
                this._drop(target);
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
            background-color: #f3f5f7
            .menu-item
                display: table
                width: 56px
                height: 54px
                padding: 0 12px
                font-size: 12px
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
                    font-weight: 200
                    border-1px(rgba(7, 17, 27, 0.1))
                &.current
                    font-weight: 700
                    background-color: #fff
                    border-width: 0px
                    .text
                        border-none()
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
                position: relative
                display: flex
                margin: 18px
                padding-bottom: 18px;
                border-1px(rgba(7, 17, 27, 0.1))
                &:last-child
                    border-none()
                    padding-bottom: 0px
                .cartControlWrapper
                    position: absolute
                    right: 0px
                    bottom: 18px
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
