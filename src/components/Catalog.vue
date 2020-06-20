<template>
    <div>
        <div class="products" v-if="filteredItems.length !== 0">
            <product v-for="product of filteredItems" :item="product" :image="image" :key="product.id_product"/>
        </div>
        <div v-else>
            <p>К сожалению, по вашему запросу ничего не найдено =(</p>
        </div>
    </div>
</template>

<script>
import product from './Product'
export default {
    data() {
        return {
            image: 'https://placehold.it/200x150',
            products: [],
            filteredItems: []
        }
    },
    methods: {
        filterItems(str) {
            const reg = new RegExp(str, 'i')
            this.filteredItems = this.products.filter(item => reg.test(item.product_name))
        },
    },
    mounted() {
        this.$parent.getJSON('/api/products')
            .then(data => {
                this.products = data
                this.filteredItems = data
            })
    },
    components: {
        product
    }
}
</script>