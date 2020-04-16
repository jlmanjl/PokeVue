<template>
    <div class="search-bar pt-0 my-3 px-2">
        <input
            class="py-3"
            type="text"
            placeholder="Search Any Generation"
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

.search-bar input{
    text-align: center;
    background: none;
    color: rgb(255, 255, 255);
    border: none;
    border-bottom: 1px solid rgb(211, 211, 211);
    width: 100%;
    transition: 0.4s;
}

.search-bar input:focus{
    outline: none;
    border-bottom-width: 3px;
}
</style>