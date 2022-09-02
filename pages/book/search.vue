<template>
  <div>
    <v-row>
      <v-col cols="6"></v-col>
    </v-row>
    <v-row>
      <v-col></v-col>
      <v-col></v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      keyword: '',
      searchResult: []
    }
  },
  methods: {
    //クエリーストロングを作成
    async search(keyword) {
      this.searchResult = []
      const baseUrl = 'https://www.googleapis.com/books/v1/volumes?'
      const params = {
        q: `intitle:${keyword}`,
        maxResults: 40
      }
      const queryParams = new URLSearchParams(params)
      console.log(baseUrl + queryParams)
      //fetchでjsonを取得
      const response = await fetch(baseUrl + queryParams)
        .then(response => response.json())
      console.log(response)
      //必要な情報を配列(searchResult)にpush
      for (let book of response.items) {
        let title = book.volumeInfo.title
        let img = book.volumeInfo.imageLinks
        let description = book.volumeInfo.description
        this.searchResult.push({
          title: title ? title : '',
          image: img ? img.thumbnail : '',
          description: description ? description.slice(0, 40) : ''
        })
      }
    }
  }
}
</script>

<style>

</style>
