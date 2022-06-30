<template>
    <v-app-bar
        app
        color="primary"
        dark
    >
        <v-spacer></v-spacer>
        <v-btn
        color="red"
        @click="deleteAllBook()"
        >
        すべて削除
        </v-btn>
    </v-app-bar>
</template>

<script>
export default {
    methods:{
        deleteAllBook(){
            this.$emit('delete-all-book')
        }
    }
}
</script>

<style>

</style>