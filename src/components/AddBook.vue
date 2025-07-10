<template>
  <div ref="addBook">
    <h2 style="margin-top: 2rem">{{ isEdit ? "Edit Book" : "Add Book" }}</h2>

    <div style="margin-bottom: 1rem">
      <input type="text" placeholder="Title" v-model="newBook.title" />
      <div v-if="errors.title" style="color: red; font-size: 10px">{{ errors.title }}</div>
    </div>

    <div style="margin-bottom: 1rem">
      <input type="text" placeholder="Author" v-model="newBook.author" />
      <div v-if="errors.author" style="color: red; font-size: 10px">{{ errors.author }}</div>
    </div>

    <div style="margin-bottom: 1rem">
      <input type="date" v-model="newBook.date" />
      <div v-if="errors.date" style="color: red; font-size: 10px">{{ errors.date }}</div>
    </div>

    <div style="margin-top: 1rem">
      <button @click="onAddBook">{{ isEdit ? "Update Book" : "Add Book" }}</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "AddBook",
  props: ["isEdit", "book", "index"],
  emits: ["onSuccess"],

  data() {
    return {
      newBook: {
        title: "",
        author: "",
        date: "",
      },
      errors: {
        title: "",
        author: "",
        date: "",
      },
    };
  },

  methods: {
    onCheckRequired() {
      this.errors.title = this.newBook.title ? "" : "Title is required";
      this.errors.author = this.newBook.author ? "" : "Author is required";
      this.errors.date = this.newBook.date ? "" : "Date is required";

      return this.newBook.title && this.newBook.author && this.newBook.date;
    },

    onClearFields() {
      this.newBook = { title: "", author: "", date: "" };
      this.errors = { title: "", author: "", date: "" };
    },

    onAddBook() {
      if (!this.onCheckRequired()) return;

      const books = JSON.parse(sessionStorage.getItem("books")) || [];

      if (this.isEdit) {
        books[this.index] = { ...this.newBook };
      } else {
        books.push({ ...this.newBook });
      }

      sessionStorage.setItem("books", JSON.stringify(books));
      this.onClearFields();
      this.$emit("onSuccess");
    },
  },

  watch: {
    book: {
      immediate: true,
      handler(newBook) {
        if (this.isEdit) {
          this.newBook = { ...newBook };
        }
      },
    },
  },
};
</script>

<style>
input {
  font-size: 1rem;
}
</style>
