<template>
    <form class="search-form" action="#" @submit.prevent="$root.$refs.cata.filterItems(searchLine)">
        <input type="text" v-model.lazy="searchLine" placeholder="Search" class="search-field">
        <button class="btn-search" type="submit">
            <i class="fas fa-search"></i>
        </button>
    </form>
</template>

<script>
export default {
    data() {
        return {
            searchLine: ''
        }
    }
}
</script>