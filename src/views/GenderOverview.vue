<template>
    <div>
        <h1 class="wrapper">{{pageTitle}}</h1>
        <ul class="wrapper item-grid">
            <li v-for="product in productsByGender" :key="product.id" 
            class="item-grid__item">
            <router-link :to="{name : 'product', params: { id: product.id}}">
                <img class="product-image" alt="" :src="makeImagePath(product)">
                <p class="product-title">{{product.name}}</p>
                <p><em>${{product.price}}</em></p>
            </router-link>
            </li>
        </ul>
        <!-- 随机推荐 -->
        <div class="wrapper random-items-wrapper">
            <h2>我们的建议</h2>
            <p>试试这个尺寸!</p>
            <section class="random-items">
                <router-link :to="{ name: 'product', params: { id: randomTop.id}}" class="random-items__item">
                     <img class="product-image" :src="makeImagePath(randomTop)" alt="">
                     <p class="product-title">{{ randomTop.name }}</p>
                     <p><em>${{ randomTop.price }}</em></p>
                </router-link>
                <router-link :to="{ name: 'product', params: { id: randomBottom.id}}" class="random-items__item">
                    <img class="product-image" :src="makeImagePath(randomBottom)" alt="">
                    <p class="product-title">{{ randomBottom.name }}</p>
                    <p><em>${{ randomBottom.price }}</em></p>
                </router-link>
                <router-link :to="{ name: 'product', params: { id: randomFootwear.id}}" class="random-items__item">
                    <img class="product-image" :src="makeImagePath(randomFootwear)" alt="">
                    <p class="product-title">{{ randomFootwear.name }}</p>
                    <p><em>${{ randomFootwear.price }}</em></p>
                </router-link>
          </section>
            <button class="btn btn--grey" @click="recommendRandomOutfit">推荐</button>
        </div>
    </div>
</template>
<script>
import {imagePath} from '@/mixins/imagePath.js'
export default {
    name: "genderOverview",
    mixins: [imagePath],
    data(){
        return{
            randomTopId: null,
            randomBottomId: null,
            randomFootwearId: null
        }
    },
    computed: {
        // 获取传进来的标题内容
        gender(){
            return this.$route.params.gender
        },
        // 首字母大写，然后进行裁剪
        pageTitle(){
            return `${this.gender[0].toUpperCase()}${this.gender.slice(1)}`
        },
        //这个特殊的getter将 性别 计算属性作为参数。
        productsByGender(){
            return this.$store.getters.productsByGender(this.gender)
        },


        randomTop() {
            return this.$store.getters.product(this.randomTopId)
        },
        randomBottom() {
             return this.$store.getters.product(this.randomBottomId)
        },
        randomFootwear() {
             return this.$store.getters.product(this.randomFootwearId)
        }
    },
    methods:{
        randomProductIdByCategory(category){
            let allProductsIncategory = this.productsByGender.filter( p=> p.category === category);
            let randomIndex = Math.floor(Math.random() * allProductsIncategory.length);
            return allProductsIncategory[randomIndex].id;
        },
          recommendRandomOutfit() {
             this.randomTopId = this.randomProductIdByCategory('Shirts');
             this.randomBottomId = this.randomProductIdByCategory('Pants');
             this.randomFootwearId = this.randomProductIdByCategory('Shoes');
        }
    },
    created(){
            console.log("1")
            this.randomTopId = this.randomProductIdByCategory('Shirts');
            this.randomBottomId = this.randomProductIdByCategory('Pants');
            this.randomFootwearId = this.randomProductIdByCategory('Shoes')

            this.recommendRandomOutfit();
    }
}
</script>

<style lang="scss">
    .random-items-wrapper {
        background: #fafafa;
        text-align: center;
        padding: 3rem;
    }
    .random-items {
        display: flex;
        align-items: center;
        justify-content: space-between;
    }
    .random-items__item {
        flex: 0 0 33%;
    }
    .item-grid{
        list-style: none;
        padding-left: 0;
        display: flex;
        align-self: flex-start;
        justify-content: space-between;
        flex-wrap: wrap;
    }
    .item-grid__item{
        box-sizing: border-box;
        text-align: center;
        padding: 1rem;
        flex:  0 0 33.3%;
        @media only screen and (max-width:832px){
            flex: 0 0 50%;
        }
        @media only screen and (max-width: 475px){
            flex: 0 0 100%;
        }
    }
</style>

