<template>
    <div class="search-bar nes-field pt-0 my-2 px-2">
        <input
            class="nes-input nes-container is-rounded"
            type="text"
            placeholder="Search..."
            v-model="searchQuery"
            @keyup="updateQuery"

             >
    </div>
</template>

<script>
export default {
    name: "SearchBar",
    data () {
        return {
            searchQuery: '',
        }
    },
    methods: {
        updateQuery() {
            this.$emit('updateQuery', this.searchQuery.toLowerCase());
        }
    }
}
</script>

<style>
.search-bar {
    margin-bottom: 16px;
    margin-top: 24px;
    background: none;
}

/* .search-bar input{
    text-align: center;
    background: white;
    border-radius: 24px;
    font-size: 18px;
    outline-width: 3px;
    border: none;
    border-bottom: 5px rgb(255, 255, 255);
} */
</style>