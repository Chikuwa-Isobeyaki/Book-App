<template>
  <div>
    <NuxtChild 
    :books="books"
    @add-book-list="addBook"
    />
  </div>
</template>

<script>
const STORAGE_KEY = 'books'
export default {
  data() {
    return {
      books: [],
      newBook: null
    }
  },
  mounted() {
    if (localStorage.getItem(STORAGE_KEY)) {
      try {
        this.books = JSON.parse(localStorage.getItem(STORAGE_KEY));
      } catch (e) {
        localStorage.removeItem(STORAGE_KEY);
      }
    }
  },
  methods: {
    addBook(e) {
      // 実際に何かしたことを入力する
      this.books.push({
        id: this.books.length,
        title: e.title,
        description: e.description,
        readDate: '',
        memo: ''
      });
      this.saveBooks();

      // (-1)[0]とすることにより配列の後ろから0番目を取得となる
      // console.log(this.books.slice(-1)[0].id)

      this.goToEditPage(this.books.slice(-1)[0].id)
    },
    removeBook(x) {
      this.books.splice(x, 1);
      this.saveBooks();
    },
    saveBooks() {
      const parsed = JSON.stringify(this.books);
      localStorage.setItem(STORAGE_KEY, parsed);
    },
    goToEditPage(id) {
      this.$router.push(`/edit/${id}`)
    }
  }
}
</script>

<style>

</style>
