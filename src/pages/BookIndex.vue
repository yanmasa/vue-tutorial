<template>
<div>
    <v-row class="mt-3">
        <v-btn color="secondary" to="/search">本を探しに行く</v-btn>
    </v-row>
    <v-row>
        <v-col cols="12"  md="6" lg="3" xl="2" v-for="(book, index) in books" :key="book.index">
            <v-card
                class="mx-auto my-3"
                max-width="344" 
            >
                <v-img
                :src="book.image"
                height="300px"
                position="top"
                ></v-img>

                <v-card-title>
                {{book.title}}
                </v-card-title>

                <v-spacer></v-spacer>

                <div>
                    <v-divider></v-divider>
                    <v-card-text>
                        {{book.description || '説明はありません。'}}
                    </v-card-text>
                </div>

                <v-card-actions align-items="center">
                    <v-btn
                        fab dark color="indigo"
                        :to='editUrl(index)'
                    >
                        <v-icon dark> mdi-pencil-plus</v-icon>
                    </v-btn>
                    <v-btn
                        fab dark color="indigo"
                        @click="deleteBook(index)"
                    >
                        <v-icon dark> mdi-minus</v-icon>
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-col>
    </v-row>
</div>
</template>

<script>

export default {
    props:{
        books: Array
    },
    methods:{
        editUrl(id){
            return `/edit/${id}`
        },
        deleteBook(index){
            this.$emit('remove-book', index)
        }
    }
}
</script>

<style>

</style>