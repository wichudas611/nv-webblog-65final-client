<template>
    <div class="pad bg-dark" width="100%" heigth="100%">
        <div class="container center">
            <h1 class="center white">Edit</h1>
            <div class="jumbotron center bg-light">
                <form v-on:submit.prevent="editBook">
                    <p class="left">
                        Metaverse
                        <input class="form-control " type="text" v-model="book.title" />
                    </p>
                    <p class="left">
                        Mark zuckerberg
                        <input class="form-control " type="text" v-model="book.author" />
                    </p>
                    <p class="left">
                        550
                        <input class="form-control " type="text" v-model="book.total_page" />
                    </p>
                    <p class="left">
                        Chula Book
                        <input class="form-control " type="text" v-model="publisher" />
                    </p>
                    <p class="left">
                        Text Book
                        <input class="form-control " type="text" v-model="category" />
                    </p>
                    <p class="left">
                        150.0
                        <input class="form-control " type="text" v-model="price" />
                    </p>
                    <p>
                        <button class="btn btn-primary btn-lg" type="submit">แก้ไข</button>
                    </p>
                </form>
            </div>
        </div>
    </div>
</template>
<script>
import BookService from "@/services/BookService";
export default {
    data() {
        return {
            book: {
                title: "",
                author: "",
                total_page: "",
                publisher: "",
                category: "",
                price: "",
            },
        };
    },
    methods: {
        async editBook() {
            try {
                await BookService.put(this.book);
                this.$router.push({
                    name: "book",
                });
            } catch (err) {
                console.log(err);
            }
        },
    },
    async created() {
        try {
            let bookId = this.$route.params.bookId;
            this.book = (await BookService.show(bookId)).data;
        } catch (error) {
            console.log(error);
        }
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

.pad {
    padding: 3% 20%;
}

.left {
    text-align: left;
}
</style>