<template>
    <form class="filter-form" method="GET">
        <div>
            <select id="subject" name="subject" @change="submitForm()">
                <option value="">Все предметы</option>
                <option v-for="subject of subjects" :key="subject" :value="subject" v-bind:selected="filters.subject == subject">{{subject}}</option>
            </select>
        </div>
        <div>
            <select id="genre" name="genre" @change="submitForm()">
                <option value="">Все жанры</option>
                <option v-for="genre of genres" :key="genre" :value="genre" v-bind:selected="filters.genre == genre">{{genre}}</option>
            </select>
        </div>
        <div>
            <select id="grade" name="grade" @change="submitForm()">
                <option value="">Все классы</option>
                <option v-for="grade of grades" :key="grade" :value="grade" v-bind:selected="filters.grade == grade">{{grade}}</option>
            </select>
        </div>
        <div>
            <input id="search" name="search" type="text" v-model="filters.search" placeholder="Поиск" class="filter-field">
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
                search: '',
                genre: '',
                grade: Number,
                subject: ''
            },
            genres: [],
            grades: [],
            subjects: []
        }
    },
    methods: {
        getTarget(e) {
            return e.target
        },
        submitForm() {
            document.querySelector('.filter-form').submit()
        }
    }
}
</script>