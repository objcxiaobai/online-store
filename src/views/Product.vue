<template>
    <section class="wrapper">
        <div class="flex-col">
            <img class="flex-col--2" :src="makeImagePath(product)" alt="">
            <div class="flex-col--2">
                <h2>{{product.name}}</h2>
                <button @click="addToCart" class="btn btn--grey">添加购物车</button>
                <p>价格: ${{product.price}}</p>
                <p>大小: {{product.size}}</p>
                <p>颜色: {{product.color}}</p>
                <p><em>还剩下 {{product.quantity}} 件</em></p>
                <h3>信息</h3>
                <ul>
                    <li>材料: {{product.details.material}}</li>
                    <li>尺寸: {{product.details.fit}}</li>
                    <li>保护: {{product.details.maintenance}}</li>
                    <li v-if="product.details.additional">额外: {{product.details.additional}}</li>
                </ul>
            </div>
        </div>
    </section>
</template>
<script>
import {imagePath} from '@/mixins/imagePath.js'
export default {
    name: "product",
    mixins: [imagePath],
    data(){
        return{
            // 获取对应产品但id,加载它但信息
            product: this.$store.getters.product(this.$route.params.id)
        }
    },
    methods: {
        addToCart(){
            // 发送一个名为“addToCart”的操作。
            this.$store.dispatch('addToCart',this.$route.params.id);
        }
    },
    created(){
        // 一直会调用这个钩子函数，so，这个组件它不会被复用
        console.log(this.$route.params.id)
    }
}
</script>

<style lang="scss" >
.flex-col{
    display: flex;
    align-items: flex-start;
}
.flex-col--2{
    width: 50%;
}

.btn {
    padding:  .5rem .75rem;
    border-radius: 3px;
    border: none;
    background-color: transparent;
    font-size: .9rem;
    font-weight: bold;
    cursor: pointer;
    transform:  all .15s ease;
}
.btn--grey{
    background-color: #2c3e50;
    color: #FFF;
    &:hover,&:focus{
        background-color: #42b983;
    }
}
</style>
