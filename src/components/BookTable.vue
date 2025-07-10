<template>
  <div>
    <div class="header">
      <h2 style="margin-right: 2rem">Book Inventory</h2>
      <button style="margin-left: auto" @click="onAddBook" v-if="!isAdd">Add Book</button>
      <button style="margin-left: auto" @click="onCancel" v-else>Cancel</button>
    </div>

    <table border="1" cellpadding="10">
      <thead>
        <tr>
          <th>Title</th>
          <th>Author</th>
          <th>Date</th>
          <th>Actions</th>
        </tr>
      </thead>

      <tbody>
        <tr v-if="books.length > 0" v-for="(book, index) in books" :key="index">
          <td>{{ book.title }}</td>
          <td>{{ book.author }}</td>
          <td>{{ book.date }}</td>
          <td class="actions">
            <span @click="onEditBook(index)">Edit</span> |
            <span @click="onDeleteBook(index)">Delete</span>
          </td>
        </tr>
        <tr v-else>
          <td colspan="4" style="text-align: center; color: red">No books available</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "BookTable",

  props: ["isAdd", "isAdded"],

  emits: ["onAddBook", "isRefresh", "onEditBook"],

  data() {
    return {
      books: [],
    };
  },

  created() {
    this.fetchBooks();
  },

  methods: {
    fetchBooks() {
      this.books = sessionStorage.getItem("books") ? JSON.parse(sessionStorage.getItem("books")) : [];
      this.$emit("isRefresh");
    },

    onAddBook() {
      this.$emit("onAddBook");
    },
    onCancel() {
      this.$emit("onCancel");
    },

    onEditBook(index) {
      const book = this.books[index];
      this.$emit("onEditBook", book, index);
    },

    onDeleteBook(index) {
      if (confirm("Are you sure you want to delete this book?")) {
        this.books.splice(index, 1);
        sessionStorage.setItem("books", JSON.stringify(this.books));
        this.fetchBooks();
      }
    },
  },

  watch: {
    isAdded(val) {
      if (val) {
        this.fetchBooks();
      }
    },
  },
};
</script>

<style>
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

table {
  width: 100%;
  margin-top: 20px;
}

button {
  background-color: #04aa6d; /* Green */
  border: none;
  color: white;
  padding: 10px 25px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  border-radius: 4px;
  cursor: pointer;

  &:hover {
    background-color: #45a049; /* Darker green */
  }
  &:active {
    background-color: #3e8e41; /* Even darker green */
  }
}

.actions span {
  cursor: pointer;
  color: blue;
  text-decoration: underline;

  &:hover {
    color: red;
  }
}
</style>
