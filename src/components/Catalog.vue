<template>
    <div>
        <div class="products" v-if="filteredItems.length !== 0">
            <ul>
                <product v-for="product of filteredItems" :item="product" :key="product.id_product"/>
            </ul>
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
            products: [],
            filteredItems: []
        }
    },
    methods: {
        filterItems(str, arr = 'products') {
            if (str == '') {
                this.filteredItems = this.products
            } else {
                const num = +str
                let reg = new RegExp(str, 'i')
                let filter = this[arr].filter(item => {
                    if (num === num) {
                        if (item.grade.length > 2) {
                            let grade = item.grade.split(';')
                            grade.forEach((el, i) => {grade[i] = parseInt(el)})
                            if (grade.indexOf(num) != -1) {
                                return 1
                            } else {
                                return 0
                            }
                        } else if (+item.grade == num) {
                            return 1
                        } else {
                            return 0
                        }
                    } else if (reg.test(item.genre)) {
                        return 1
                    } else if (reg.test(item.subject)) {
                        return 1
                    } else { return 0}
                })
                this.filteredItems = filter
            }
        },
    },
    mounted() {
        this.$parent.postJSON()
            .then(data => {
                this.products = data.items
                this.filteredItems = data.items
                let grades = this.$root.$children[0].$refs.filter.grades,
                    genres = this.$root.$children[0].$refs.filter.genres,
                    subjects = this.$root.$children[0].$refs.filter.subjects

                data.items.forEach(el => {
                    if (genres.indexOf(el.genre) == -1) {
                        genres.push(el.genre)
                    }
                    if (subjects.indexOf(el.subject) == -1) {
                        subjects.push(el.subject)
                    }
                    if (el.grade.length > 2) {
                        let grade = el.grade.split(';')
                        grade.forEach(item => {
                            if (grades.indexOf(item) == -1) {
                                grades.push(item)
                            }
                        })
                    } else {
                        if (grades.indexOf(el.grade) == -1) {
                            grades.push(el.grade)
                        }
                    }
                })
                grades.sort((a, b) => +a > +b ? 1 : -1)
                subjects.sort((a, b) => a > b ? 1 : -1)
                genres.sort((a, b) => a > b ? 1 : -1)
            })
    },
    components: {
        product
    }
}
</script>