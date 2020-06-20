<template>
    <div class="product-item">
        <img :src="image" alt="Some img">
        <div class="desc">
            <h3>{{item.product_name}}</h3>
            <p>{{item.price}} $</p>
            <button class="buy-btn" @click="$root.$refs.cart.addProduct(item)">Купить</button>
        </div>
    </div>
</template>

<script>
export default {
    props: ['image', 'item']
}
</script>