<template >
    <div class="bg-dark">
      <nav class="navbar navbar-light bg-light">
        <button class="navbar-brand btn btn-outline" v-on:click="navigateTo('/IndexAdmin')">Book</button>
        
          <button class="btn btn-outline-danger my-2 my-sm-0" v-on:click="logout" type="submit">
            ออกจากระบบ
          </button>
  
      </nav>
      <br />
      <div class="container">
        <h1 class="center white">หนังสือทั้งหมด</h1>
        <h4 class="center white">
          จํานวนหนังสือทั้งหมด {{ books.length }} เล่ม
          <button class="btn btn-primary" v-on:click="navigateTo('/book/create')">
            เพิ่มหนังสือ
          </button>
        </h4>
  
        <div v-for="book in books" v-bind:key="book.id">
          <div class="jumbotron center bg-light">
            <h1 class="display-4">{{ book.title }}</h1>
            <img :src="book.pic" :alt="book.title" width="30%" height="30%" />
            <br />
            <br />
            <h4>Metaverse {{ book.title }}</h4>
            <h4>Mark zuckerberg {{ book.author }}</h4>
            <h4>550{{ book.total_page }}</h4>
            <h4>Chula Book{{ book.publisher }}</h4>
            <h4>Text Book{{ book.category}}</h4>
            <h4>150.0 {{ book.price }}</h4>
            <p>
              <button
                class="btn btn-primary btn-lg"
                v-on:click="navigateTo('/book/edit/' + book.id)"
              >
                Edit
              </button>
              <button class="btn btn-danger btn-lg" v-on:click="deleteBook(book)">
                Delete
              </button>
            </p>
          </div>
        </div>
      </div>
      <br />
    </div>
  </template>
  <script>
  import BookService from "@/services/BookService";
  export default {
    data() {
      return {
        books: [],
        BASE_URL: "http://localhost:8081/asset"
      };
    },
    async created() {
      this.books = (await BookService.index()).data;
    },
    methods: {
      navigateTo(route) {
        this.$router.push(route);
      },
      async deleteBook(book) {
        let result = confirm("Want to delete?");
        if (result) {
          try {
            await BookService.delete(book);
            this.books = (await BookService.index()).data;
          } catch (err) {
            console.log(err);
          }
        }
      },
      logout() {
        this.$store.dispatch("setToken", null);
        this.$store.dispatch("setUser", null);
        this.$router.push({
          name: "login",
        });
      },
    },
  };
  </script> 
  <style scoped>
  .center {
    text-align: center;
  }
  .white {
    color: aliceblue;
  }
  </style>