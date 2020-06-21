<template>
    <li class="product-item">
        <div class="product-item_figure">
            <img :src="img_url()+item.courseId" :alt="item.subject">
        </div>
        <div class="product-item_info">
            <p class="product-item__title">{{item.subject}}</p>
            <p class="product-item__grade">{{trueGrade(item.grade)}} класс</p>
            <p class="product-item__genre">{{item.genre}}</p>    
            <p class="product-item__meta"><a :href="item.shopUrl">Подробнее</a></p>
            <p class="product-item__controls">
                <a href="#" class="product-item_buy-btn" v-if="$root.$children[0].currency == 'rub'">{{item.price}} рублей</a>
                <a href="#" class="product-item_buy-btn" v-else>{{item.priceBonus}} бонусов</a>
            </p>
        </div>
    </li>
</template>

<script>
export default {
    props: ['item'],
    data() {
        return {
            base_url: 'https://www.imumk.ru/',
            img_url: () => this.base_url+'svc/coursecover/'
        }
    },
    methods: {
        trueGrade(str) {
            if (str.length <= 2) {
                return str
            } else {
                let grade = str.split(';')
                return `${grade[0]}-${grade[grade.length-1]}`
            }
        }
    }
}
</script>