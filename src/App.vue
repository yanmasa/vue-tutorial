<template>
  <v-app>
    <GlobalHeader @delete-all-book="deleteAllBook"/>
    <v-main>
      <v-container>
        <router-view
        @add-book-list="addBook"
        :books="books"
        @update-book-info="updateBook"
        @remove-book="removeBook"
        
        />
      </v-container>
    </v-main>
    <GlobalFooter />
  </v-app>
</template>

<script>
import GlobalHeader from '@/global/GlobalHeader';
import GlobalFooter from '@/global/GlobalFooter';
const STORAGE_KEY = 'books';
export default {
  name: 'App',
  components:{
    GlobalHeader,
    GlobalFooter
  },
  data(){
    return{
      books: [],
      newBook: null,
    }
  },
  mounted() {
    if (localStorage.getItem(STORAGE_KEY)) {
    try {
        this.books = JSON.parse(localStorage.getItem(STORAGE_KEY));
    } catch(e) {
        localStorage.removeItem(STORAGE_KEY);
    }
    }
},
methods: {
    addBook(e) {
        this.books.push({
          id: this.books.length,
          ...e,
          readDate: '',
          memo: ''
        });
        this.saveBooks();
        console.log(this.books.slice(-1)[0].id)
        this.goToEditPage(this.books.slice(-1)[0].id)

    },
    removeBook(x) {
        this.books.splice(x, 1);
        this.saveBooks();
    },
    deleteAllBook() {
      const isDeleted = 'LocalStorageのデータを削除してもいいですか？'
      if(window.confirm(isDeleted)){
        localStorage.setItem(STORAGE_KEY, '');
        localStorage.removeItem(STORAGE_KEY)
        this.books=[];
        window.location.reload()
      }
      this.saveBooks();
    },
    saveBooks() {
        const parsed = JSON.stringify(this.books);
        localStorage.setItem(STORAGE_KEY, parsed);
    },
    updateBook(e) {
      // 受け取ったeのparamを元のにassign、spliceで置き換え
      this.books.splice(e.id,1,Object.assign(this.books[e.id],{...e}))
      this.saveBooks();
      this.$router.push('/');
    },
    goToEditPage(id){
      this.$router.push(`/edit/${id}`)
    }
}
        
};
</script>
