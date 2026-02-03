<template>
  <v-sheet border rounded>
    <v-data-table
      v-model:page="page"
      :headers="headers"
      :items="books"
      :items-per-page="itemsPerPage"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>
            <v-icon
              color="medium-emphasis"
              icon="mdi-book-multiple"
              size="x-small"
              start
            ></v-icon>
            Popular books
          </v-toolbar-title>

          <v-btn
            class="me-2"
            prepend-icon="mdi-plus"
            rounded="lg"
            text="Add a Book"
            border
            @click="add"
          ></v-btn>
        </v-toolbar>
      </template>

      <template v-slot:item.title="{ value }">
        <v-chip
          :text="value"
          border="thin opacity-25"
          prepend-icon="mdi-book"
          label
        >
          <template v-slot:prepend>
            <v-icon color="medium-emphasis"></v-icon>
          </template>
        </v-chip>
      </template>

      <template v-slot:item.actions="{ item }">
        <div class="d-flex ga-2 justify-end">
          <v-icon
            color="medium-emphasis"
            icon="mdi-pencil"
            size="small"
            @click="edit(item)"
          ></v-icon>

          <v-icon
            color="medium-emphasis"
            icon="mdi-delete"
            size="small"
            @click="remove(item.id)"
          ></v-icon>
        </div>
      </template>

      <template v-slot:no-data>
        <v-btn
          prepend-icon="mdi-backup-restore"
          rounded="lg"
          text="Reset data"
          variant="text"
          border
          @click="reset"
        ></v-btn>
      </template>

      <template v-slot:bottom>
        <v-divider></v-divider>
        <div class="d-flex align-center justify-space-between pa-4">
          <div class="d-flex align-center" style="width: 160px">
            <span class="text-caption text-medium-emphasis me-3"
              >Rows per page:</span
            >
            <v-select
              v-model="itemsPerPage"
              :items="[5, 10, 15, 20]"
              variant="underlined"
              density="compact"
              hide-details
            ></v-select>
          </div>

          <v-pagination
            v-model="page"
            :length="pageCount"
            :total-visible="5"
            rounded="circle"
            density="comfortable"
          ></v-pagination>

          <div
            class="text-caption text-medium-emphasis"
            style="width: 160px; text-align: right"
          >
            Total: {{ books.length }} items
          </div>
        </div>
      </template>
    </v-data-table>
  </v-sheet>

  <v-dialog v-model="dialog" max-width="500">
    <v-card
      :subtitle="`${isEditing ? 'Update' : 'Create'} your favorite book`"
      :title="`${isEditing ? 'Edit' : 'Add'} a Book`"
    >
      <template v-slot:text>
        <v-row>
          <v-col cols="12">
            <v-text-field
              v-model="formModel.title"
              label="Title"
            ></v-text-field>
          </v-col>

          <v-col cols="12" md="6">
            <v-text-field
              v-model="formModel.author"
              label="Author"
            ></v-text-field>
          </v-col>

          <v-col cols="12" md="6">
            <v-select
              v-model="formModel.genre"
              :items="['Fiction', 'Dystopian', 'Non-Fiction', 'Sci-Fi']"
              label="Genre"
            ></v-select>
          </v-col>

          <v-col cols="12" md="6">
            <v-number-input
              v-model="formModel.year"
              :max="currentYear"
              :min="1"
              label="Year"
            ></v-number-input>
          </v-col>

          <v-col cols="12" md="6">
            <v-number-input
              v-model="formModel.pages"
              :min="1"
              label="Pages"
            ></v-number-input>
          </v-col>
        </v-row>
      </template>

      <v-divider></v-divider>

      <v-card-actions class="bg-surface-light">
        <v-btn text="Cancel" variant="plain" @click="dialog = false"></v-btn>
        <v-spacer></v-spacer>
        <v-btn color="primary" text="Save" variant="flat" @click="save"></v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script setup>
import { onMounted, ref, shallowRef, computed, watch } from "vue";

const currentYear = new Date().getFullYear();

// --- State ---
const books = ref([]);
const page = ref(1);
const itemsPerPage = ref(5);
const dialog = shallowRef(false);
const formModel = ref(createNewRecord());

// --- Computed ---
const isEditing = computed(() => !!formModel.value.id);

const pageCount = computed(() => {
  return Math.ceil(books.value.length / itemsPerPage.value);
});

const headers = [
  { title: "Title", key: "title", align: "start" },
  { title: "Author", key: "author" },
  { title: "Genre", key: "genre" },
  { title: "Year", key: "year", align: "end" },
  { title: "Pages", key: "pages", align: "end" },
  { title: "Actions", key: "actions", align: "end", sortable: false },
];

// --- Watchers ---
// Reset to page 1 if rows per page changes to avoid empty views
watch(itemsPerPage, () => {
  page.value = 1;
});

// --- Lifecycle ---
onMounted(() => {
  reset();
});

// --- Methods ---
function createNewRecord() {
  return {
    title: "",
    author: "",
    genre: "",
    year: currentYear,
    pages: 1,
  };
}

function add() {
  formModel.value = createNewRecord();
  dialog.value = true;
}

function edit(item) {
  // Use a shallow copy to prevent table updating before "Save" is clicked
  formModel.value = { ...item };
  dialog.value = true;
}

function remove(id) {
  const index = books.value.findIndex((book) => book.id === id);
  if (index !== -1) books.value.splice(index, 1);
}

function save() {
  if (isEditing.value) {
    const index = books.value.findIndex(
      (book) => book.id === formModel.value.id,
    );
    books.value[index] = { ...formModel.value };
  } else {
    // Generate a unique ID for new entries
    const newId = books.value.length
      ? Math.max(...books.value.map((b) => b.id)) + 1
      : 1;
    books.value.push({ ...formModel.value, id: newId });
  }

  dialog.value = false;
}

function reset() {
  dialog.value = false;
  formModel.value = createNewRecord();
  books.value = [
    {
      id: 1,
      title: "To Kill a Mockingbird",
      author: "Harper Lee",
      genre: "Fiction",
      year: 1960,
      pages: 281,
    },
    {
      id: 2,
      title: "1984",
      author: "George Orwell",
      genre: "Dystopian",
      year: 1949,
      pages: 328,
    },
    {
      id: 3,
      title: "The Great Gatsby",
      author: "F. Scott Fitzgerald",
      genre: "Fiction",
      year: 1925,
      pages: 180,
    },
    {
      id: 4,
      title: "Sapiens",
      author: "Yuval Noah Harari",
      genre: "Non-Fiction",
      year: 2011,
      pages: 443,
    },
    {
      id: 5,
      title: "Dune",
      author: "Frank Herbert",
      genre: "Sci-Fi",
      year: 1965,
      pages: 412,
    },
    {
      id: 6,
      title: "Brave New World",
      author: "Aldous Huxley",
      genre: "Dystopian",
      year: 1932,
      pages: 268,
    },
    {
      id: 7,
      title: "The Hobbit",
      author: "J.R.R. Tolkien",
      genre: "Fiction",
      year: 1937,
      pages: 310,
    },
    {
      id: 8,
      title: "The Catcher in the Rye",
      author: "J.D. Salinger",
      genre: "Fiction",
      year: 1951,
      pages: 234,
    },
  ];
}
</script>
