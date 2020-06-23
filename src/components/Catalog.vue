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
        filterItems() {
            let search = location.search.replace('?', '').split('&')
            let comp = this
            search.forEach((item) => {
                let element = item.split('=')
                element[1] = decodeURIComponent(element[1]).replace('+', ' ')
                document.querySelector(`#${element[0]}`).value = element[1]
                let reg = new RegExp(element[1], 'i')
                comp.$root.$children[0].$refs.filter.filters[element[0]] = element[1]
                if (element[1] != '') {
                    if (element[0] == 'grade') {
                        comp.filteredItems = comp.filteredItems.filter(el => {
                            if (el[element[0]].length > 2) {
                                el[element[0]].split(';').forEach(num => {
                                    if (reg.test(num)) {
                                        return 1
                                    }
                                })
                            } else if (reg.test(el[element[0]])) {
                                return 1
                            } else {
                                return 0
                            }
                        })
                    } else if (element[0] == 'search') {
                        comp.filteredItems = comp.filteredItems.filter(el => {
                            if (reg.test(el.genre)) {
                                return 1
                            } else if (reg.test(el.subject)) {
                                return 1
                            } else { return 0}
                        })
                    } else {
                        comp.filteredItems = comp.filteredItems.filter(el => reg.test(el[element[0]]))
                    }
                }
            })
        }
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
        .then(() => {
            if (location.search) {
                this.filterItems()
            }
        })
    },
    components: {
        product
    }
}
</script>