<template>
    <div class="wrapper">
        <h1>购物车</h1>
        <div class="flex-col">
        <ul class="cart-list">
            <li class="flex-col cart-list__item"  v-for="(item,index) in cartItems" :key="index">
                <img :src="makeImagePath(item)" class="thumbnail" alt="">
                <div class="flex-col cart-list__item__details">
                    <div>
                        <p>{{item.name}}</p>
                        <p>大小: {{item.size}}</p>
                        <p>颜色: {{item.color}}</p>
                    </div>
                    <p>${{item.price}}</p>
                    <button @click="removeFromCart(item.id)"
                    class="btn cart-list__btn-remove"
                    >
                    删除
                    </button>
                </div>
            </li>
        </ul>
        <section class="total-section">
            <ul class="total-section-list">
                <li class="total-section__item">
                    <p class="total-section__item__label">{{cartItemsCount}} 件商品</p>
                    <p>{{itemsSubtotal}}</p>
                </li>
                <li class="total-section__item">
                    <p class="total-section__item__label">到货时间</p>
                    <select v-model="selectedShippingOption">
                        <option disabled value="">请选择时间</option>
                        <option v-for="opthion in shippingOptionsArray" :key="opthion.text" :value="opthion.rate">
                            {{opthion.text}}(${{opthion.rate}})
                        </option>
                    </select>
                </li>
                <li class="total-section__item">
                    <p class="total-section__item__label">小计</p>
                    <p>{{subtotal}}</p>
                </li>
                <li class="total-section__item">
                    <p class="total-section__item__label">税 ({{salesTaxPercentage}})</p>
                    <p>{{salesTaxApplied}}</p>
                </li>
                <li class="total-section__item">
                    <p class="total-section__item__label">总共</p>
                    <p>{{total}}</p>
                </li>
            </ul>
            <button :disabled="!this.selectedShippingOption" 
            class="btn btn--grey total-section__checkout-button">
                提交
            </button>
        </section>
       </div> 
    </div>
</template>
<script>
import {imagePath} from '@/mixins/imagePath.js'
export default {
    name: 'cart',
    mixins: [imagePath],
    data(){
        return{
            selectedShippingOption: '',
            shippingOptionsArray:[
                {
                    text: '一天',
                    rate: 20
                },
                {
                    text: '两天',
                    rate: 15
                },
                {
                    text: '三到四天',
                    rate: 10
                },
                {
                    text: '一周',
                    rate: 5,
                }
            ],
            // 税收
            salesTax: 0.06
        }
    },
    computed: {
        cartItems(){
            return this.$store.getters.cartItems;
        },
        cartItemsCount(){
            return this.cartItems.length;
        },
        itemsSubtotal(){
            return this.cartItems.reduce((total,item) => total + item.price, 0);
        },
        subtotal(){
            if (this.selectedShippingOption){
                return Number(this.itemsSubtotal) + Number(this.selectedShippingOption);
            }

            return '---';
        },
        // 税转换百分比
        salesTaxPercentage(){
            return `${this.salesTax * 100}%`
        },
        salesTaxApplied(){
            if(this.selectedShippingOption){
                return (this.subtotal * this.salesTax).toFixed(2);
            }
            return '---';
        },
        total(){
            if(this.selectedShippingOption){
                return Number(this.subtotal) + Number(this.salesTaxApplied);
            }
            return '---';
        }
    },
    methods:{
        // 向store 发送删除操作
        removeFromCart(itemId){
            this.$store.dispatch('removeFromCart',itemId)
        }
    }
}
</script>

<style lang="scss">
    .cart-list{
        width: 70%;
        margin-right: 1rem;
        @media only screen and (max-width: 832px) {
            width: 100%;
        }
    }
    .cart-list__item{
        width: 100%;
        border-bottom: 1px solid #2c3e50;
    }
    .cart-list__item__details{
        flex: 2;
        justify-content: space-between;
        margin-left: 2rem;
    }
    .cart-list__btn-remove{
        margin-top: .5rem;
        &:hover{
            color: red;
        }
    }
    .thumbnail{
        max-width: 100px;
        margin-top: .5rem;
    }

    .total-section{
        background: #FAFAFA;
        padding: 0 1rem 1rem;
        //min-width: 33%;
    }

    .total-section-list{
        margin: 0;
    }
    .total-section__item{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .total-section__item__label{
        font-weight: bold;
        margin-right: 1rem;
    }
    .total-section__checkout-button{
        width: 100%;
    }
</style>

