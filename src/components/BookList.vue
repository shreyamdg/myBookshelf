<template>
  <div class="container">
    <div class="row row-cols-md-5 g-4">
      <div class="col" v-for="book in books" :key="book.id">
        <div class="card">
          <div class="card-body">
            <div class="front">
              <img :src="getThumbnail(book)" class="card-img-top" alt="Book cover">
              <h5 class="card-title text-center mt-2">{{ truncatedTitle(book) }}</h5> 
            </div>
            <div class="back">
              <h5 class="card-title">{{ getTitle(book) }}</h5>
              <p class="card-text">Authors: {{ getAuthors(book) }}</p>
              <p class="card-text">Published Date: {{ getPublishedDate(book) }}</p>
              <button @click="openModal(book)" class="btn btn-primary d-block mx-auto">View Description</button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <BookDescriptionModal v-if="showModal" :book="selectedBook" :showModal="showModal" @close="closeModal" />
  </div>
</template>

<script>
import BookDescriptionModal from './BookDescriptionModal.vue';
import imageNotAvailable from '../assets/img/image_not_available.png';

export default {
  components: {
    BookDescriptionModal
  },
  props: {
    books: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      selectedBook: null,
      showModal: false,
    };
  },
  methods: {
    openModal(book) {
      this.selectedBook = book;
      this.showModal = true;
    },
    closeModal() {
      this.selectedBook = null;
      this.showModal = false;
    },
    getThumbnail(book) {
      if (book && book.volumeInfo && book.volumeInfo.imageLinks && book.volumeInfo.imageLinks.thumbnail) {
        return book.volumeInfo.imageLinks.thumbnail;
      }
      return imageNotAvailable;
    },
    getTitle(book) {
      if (book && book.volumeInfo && book.volumeInfo.title) {
        return book.volumeInfo.title;
      }
      return '';
    },
    truncatedTitle(book) {
      const title = this.getTitle(book);
      if (title.length > 50) {
        return title.slice(0, 50) + '...';
      }
      return title;
    },
    getAuthors(book) {
      if (book && book.volumeInfo && book.volumeInfo.authors && book.volumeInfo.authors.length > 0) {
        return book.volumeInfo.authors.join(', ');
      }
      return 'Unknown Author';
    },
    getPublishedDate(book) {
      if (book && book.volumeInfo && book.volumeInfo.publishedDate) {
        return book.volumeInfo.publishedDate;
      }
      return 'Unknown Date';
    }
  }
};
</script>


<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Alegreya:wght@400;700&display=swap');

.card {
  font-family: 'Alegreya', serif;
  transition: transform 1s ease-in-out;
  height: 100%;
  width: 100%;
  background-color: #f8f9fa; 
  border: 1px solid #ced4da; 
  border-radius: 10px; 
  perspective: 1000px; 
}

.card:hover {
  transform: rotateY(180deg);
}

.front,
.back {
  height: 100%;
}

.front {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.front .card-title {
  text-align: center; 
}

.front img {
  margin-bottom: 10px; 
}

.back {
  display: none;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start; 
}

.card:hover .front {
  display: none;
}

.card:hover .back {
  display: flex;
  transform: rotateY(180deg);
}
</style>
