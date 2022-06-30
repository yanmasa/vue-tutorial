<template>
    <div>
        <v-row>
            <v-col md="6">
                <v-text-field v-model="keyword" label="本を検索"></v-text-field>
            </v-col>
            <v-col class="mt-3">
                <v-btn color="primary" @click.prevent="search(keyword)">検索</v-btn>
                <v-btn color="secondary" to="/" class="mx-2">戻る</v-btn>
            </v-col>
        </v-row>

        <v-row>
            <v-col cols="12"  md="6" lg="3" xl="2" v-for="(item, index) in searchResult" :key="item.index">
                <v-card
                    class="mx-auto my-3"
                    max-width="344" 
                >
                    <v-img
                    :src="item.image"
                    height="300px"
                    position="top"
                    ></v-img>

                    <v-card-title>
                    {{item.title}}
                    </v-card-title>

                    <v-spacer></v-spacer>

                    <div>
                        <v-divider></v-divider>
                        <v-card-text>
                            {{item.description || '説明はありません。'}}
                        </v-card-text>
                    </div>

                    <v-card-actions align-items="center">
                        <v-btn
                            fab dark color="indigo"
                            @click="addBookList(index)"
                        >
                            <v-icon dark> mdi-plus</v-icon>
                        </v-btn>
                    </v-card-actions>
                </v-card>
            </v-col>
        </v-row>
    </div>
</template>

<script>
export default {
    name: 'BookSearch',
    data(){
        return {
            keyword:'',
            searchResult:[],
        }
    },
    methods:{
        addBookList(index){
            this.$emit('add-book-list', this.searchResult[index])
        },
        async search(keyword){
            this.searchResult = []
            const baseUrl = 'https://www.googleapis.com/books/v1/volumes?'
            const params = {
                q: `intitle:${keyword}`,
                maxResults:40,
            }
            const queryParams = new URLSearchParams(params)
            console.log(baseUrl + queryParams)

            const response = await fetch(baseUrl + queryParams)
            .then( response => response.json())
            .catch(e => console.log(e))
            console.log(response.items)

            for(let book of response.items ){
                let title = book.volumeInfo.title
                let img = book.volumeInfo.imageLinks
                let description = book.volumeInfo.description
                this. searchResult.push({
                    title: title ? title : '',
                    image: img ? img.thumbnail : '',
                    description: description ? description.slice(0, 40) : '',
                })
            }
            console.log(this.searchResult)
        }
    }
}
</script>

<style>
    .v-card__text{
        min-height: 98px;
    }
    .v-card__title{
        min-height: 128px;
        font-size: 3em;
        align-items: center;
    }
    .v-card__actions {
        justify-content: center;
    }
</style>