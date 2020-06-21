<template>
    <form class="filter-form" action="#" @submit.prevent="$root.$children[0].$refs.cata.filterItems(filters.searchLine)">
        <div>
            <select id="subj" name="subj" @change="$root.$children[0].$refs.cata.filterItems(getTarget($event), 'filteredItems')">
                <option value="">Все предметы</option>
                <option v-for="subject of subjects" :key="subject" :value="subject">{{subject}}</option>
            </select>
        </div>
        <div>
            <select id="genre" name="genre" @change="$root.$children[0].$refs.cata.filterItems(getTarget($event), 'filteredItems')">
                <option value="">Все жанры</option>
                <option v-for="genre of genres" :key="genre" :value="genre">{{genre}}</option>
            </select>
        </div>
        <div>
            <select id="grade" name="grade" @change="$root.$children[0].$refs.cata.filterItems(getTarget($event), 'filteredItems')">
                <option value="">Все классы</option>
                <option v-for="grade of grades" :key="grade" :value="grade">{{grade}}</option>
            </select>
        </div>
        <div>
            <input type="text" v-model.lazy="filters.searchLine" placeholder="Поиск" class="filter-field">
            <button class="filter-btn" type="submit">
                <i class="fas fa-search"></i>
            </button>
        </div>
    </form>
</template>

<script>
export default {
    data() {
        return {
            filters: {
                searchLine: '',
                genres: '',
                grades: '',
                subjects: ''
            },
            genres: [],
            grades: [],
            subjects: []
        }
    },
    methods: {
        getTarget(e) {
            return e.target
        }
    }
}
</script>