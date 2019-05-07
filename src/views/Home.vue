<template>
  <div>
    <section class="wrapper">  
      <h2>精选商品</h2>
      <ul class="featured-items">
        <li v-for="product in featuredProducts" :key="product.id" class="featured-items__item">
         <!-- 进入产品详细页面,并且进行传参 -->
         <router-link :to="{ name : 'product' , params: {id : product.id}}" class="router-a">
          <img :src="makeImagePath(product)" alt="" class="product-image">
          <p class="product-title">{{product.name}}</p>
          <p><em>${{product.price}}</em></p>
         </router-link>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
// @ is an alias to /src
import {imagePath} from '@/mixins/imagePath.js'
export default {
  name: "home",
  mixins: [imagePath],
  computed:{

    featuredProducts(){
      //访问vuex 存储的数据，并且给出范围
      return this.$store.getters.featuredProducts.slice(0,3)
    }
  }

};
</script>

<style lang="scss">

.featured-items{
  padding-left: 0;
  list-style: none;
  display:  flex;
  justify-content: space-between;
  @media only screen and (max-width: 832px) {
    flex-direction: column;
  }
}

.featured-items__item{
  width: 33%;
  text-align: center;
  @media only screen and (max-width: 832px){
    width: 100%;
  }
}

.product-image{
  max-height: 200px;
}

.product-title{
  font-weight: bold;
}

.router-a{
  text-decoration: none;
}

</style>

