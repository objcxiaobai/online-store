<template>
  <div id="app">
    <div id="nav" class="wrapper flex-col flex-col--align-center">
      <h1 class="flex-col--2">线上商店</h1>
      <div class="flex-col--2 nav-items">
          <router-link to="/" class="nav-items__item">首页</router-link>
          <router-link :to="{name: 'gender-overview', params: {gender: 'women'}}"
          class="nav-items__item">女性</router-link>          
          <router-link :to="{name: 'gender-overview', params: {gender: 'men'}}"
          class="nav-items__item">男性</router-link>
          <router-link to="/cart" class="nav-items__item">
          购物车
          <counter-badge :count="cartCount"></counter-badge>
          </router-link>
      </div>
    </div>
    <router-view />
  </div>
</template>

<script>
import CounterBadge from '@/components/CounterBadge';
export default {
    name: 'app',
    components:{
      CounterBadge
    },
    computed:{
      cartCount(){
        return this.$store.state.cart.length;
      }
    }
}
</script>


<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
#nav {
  padding: 30px;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
.wrapper{
  max-width: 800px;
  margin: 0  auto;
  @media only screen and (max-width: 832px){
    max-width: 100%;
    padding: 1rem;
    text-align: center;
  }
}

.nav-items{
  justify-content: flex-end;
  display: flex;
  margin-top: 2rem;

}
.nav-items__item{
  margin-left: 1rem;
  position: relative;
  text-decoration: none;
}
ul{
  padding-left: 0;
  list-style: none;
}
</style>
