<template>
  <main>
    <BookTable @onAddBook="isAdd = true" :isAdd="isAdd" @onCancel="isAdd = false" :isAdded="isAdded" @isRefresh="isAdded = false" @onEditBook="onEditBook" />
  </main>
  <div v-if="isAdd || isEdit">
    <AddBook @onSuccess="onSuccess" :isEdit="isEdit" :book="book" :index="index" />
  </div>
</template>

<script>
import BookTable from "./components/BookTable.vue";
import AddBook from "./components/AddBook.vue";

export default {
  components: {
    BookTable,
    AddBook,
  },

  data() {
    return {
      isAdd: false,
      isEdit: false,
      isAdded: false,
      book: {},
      index: null,
    };
  },

  methods: {
    onSuccess() {
      this.isAdd = false;
      this.isAdded = true;
      this.isEdit = false;
      this.book = {};
      this.index = null;
    },

    onEditBook(book, index) {
      this.isEdit = true;
      this.book = book;
      this.index = index;
    },
  },
};
</script>
