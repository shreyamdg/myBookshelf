<template>
  <!-- <main> -->
    <section class="hero">
      <div class="hero-content">
        <div class="search-container">
          <p class="hero-title">myBookshelf</p>
          <p class="hero-desc">Discover a wide selection of books and explore the world of literature</p>
          <BookSearch @search="performSearch" />
        </div>
      </div>
    </section>

    <div v-if="books !== null" ref="resultsSection">
      <header>
        <div class="results-header">
          <h3 style="font-family: 'Alegreya', sans-serif;">Results: {{ totalBooks }}</h3>
          <div class="header-controls">
            <button @click="toggleFilters" style="border-radius: 6px;">
              {{ filtersVisible ? 'Hide Filters' : 'Show Filters' }}
            </button>
            <select id="sortSelect" v-model="selectedSort" class="sort-select" @change="handleSortChange">
              <option value="relevance">Relevance</option>
              <option value="newest">Newest</option>
            </select>
          </div>
        </div>

        <Transition>
          <div v-show="filtersVisible" class="header-filters">
            <div class="button-group">
              <button @click="applyFilter('partial')" title="Returns results where at least parts of the text are previewable.">Partial Text</button>
              <button style="margin-right: 10px;" @click="applyFilter('full')" title="Only returns results where all of the text is viewable.">Full Text</button>
              <button style="margin-right: 10px;" @click="applyFilter('free-ebooks')" title="Only returns results that are free Google eBooks.">Free eBooks</button>
              <button style="margin-right: 10px;" @click="applyFilter('paid-ebooks')" title="Only returns results that are Google eBooks with a price.">Paid eBooks</button>
              <button style="margin-right: 10px;" @click="applyFilter('ebooks')" title="Only returns results that are Google eBooks, paid or free.">All eBooks</button>
            </div>
          </div>
        </Transition>
      </header>

      <TransitionGroup name="list" class="results-wrapper" ref="myBooks">
        <p class="info-text">
          Position your cursor over a book and flip it to reveal its details. 
          Should the book be available for purchase, an option to buy will appear once you click on "View Description." 
          In cases where the book is available for free download, you will be presented with an option to download it directly.
        </p>
        <BookList :books="books"/>
      </TransitionGroup>
      <BookPagination :total="totalBooks" :current-page="currentPage" :per-page="perPage" @page-change="handlePageChange" class="pagination-space" />
      <br>
      <a href="#" class="scroll-to-top" @click="scrollToTop">Scroll to Top</a>
    </div>

    <div v-else class="image-container">
      <img src="../assets/img/brand-curious-cat.png" alt="" width="200px" height="200px">
      <div class="placeholder-container">
        <p class="search-placeholder">{{ currentText }}</p>
      </div>
    </div>
    <BookFooter />
  <!-- </main> -->
</template>

<script>
import axios from 'axios';
import BookList from '../components/BookList.vue';
import BookSearch from '../components/BookSearch.vue';
import BookPagination from '../components/BookPagination.vue';
import BookFooter from '../components/BookFooter.vue';

export default {
  components: {
    BookList,
    BookSearch,
    BookPagination,
    BookFooter
  },
  data() {
    return {
      books: null,
      totalBooks: 0,
      currentPage: 1,
      perPage: 10,
      searchQuery: '',
      isEnglish: true,
      englishText: "Enter the book that you want to find..",
      latinText: "Inscribe librum quem quaeris invenire..",
      filtersVisible: false,
      query: '',
      selectedSort: 'relevance'
    };
  },
  created() {
    this.toggleText();
  },
  methods: {
    fetchBooks() {
      // Calculate startIndex based on currentPage and perPage
      const startIndex = (this.currentPage - 1) * this.perPage;
      
      // Construct the API URL with startIndex and maxResults parameters
      let url = `https://www.googleapis.com/books/v1/volumes?q=${this.searchQuery}&orderBy=${this.selectedSort}&startIndex=${startIndex}&maxResults=${this.perPage}`;

      // Make the network request
      axios.get(url)
        .then(response => {
          this.totalBooks = response.data.totalItems;
          this.books = response.data.items;

          // Set a timer since the DOM update takes a while in this case
          setTimeout(() => {
            this.$nextTick(() => {
              const resultsSection = this.$refs.resultsSection;
              if (resultsSection && resultsSection.offsetHeight > 0) {
                  resultsSection.scrollIntoView({ behavior: "smooth"});
              }
            });
          }, 200);
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
    },
    applyFilter(filter) {
      this.currentPage = 1;

      let url = `https://www.googleapis.com/books/v1/volumes?q=${this.searchQuery}&filter=${filter}&orderBy=${this.selectedSort}`;
      axios.get(url)
        .then(response => {
          this.totalBooks = response.data.totalItems;
          this.books = response.data.items;
          setTimeout(() => {
            this.$nextTick(() => {
              const resultsSection = this.$refs.resultsSection;
              if (resultsSection && resultsSection.offsetHeight > 0) {
                  resultsSection.scrollIntoView({ behavior: "smooth"});
              }
            });
          }, 200);
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
    },
    handleSortChange() {
      // Reset current page to 1 when sorting changes
      this.currentPage = 1;
      this.fetchBooks();
    },
    performSearch(query) {
      this.searchQuery = query;
      this.fetchBooks();
    },
    handlePageChange(pageNumber) {
      this.currentPage = pageNumber;
      this.fetchBooks();
    },
    toggleText() {
      setInterval(() => {
        this.isEnglish = !this.isEnglish;
      }, 5000);
    },
    toggleFilters() {
      this.filtersVisible = !this.filtersVisible;
    },
    scrollToTop() {
      window.scrollTo({
        top: 0,
        behavior: "smooth"
      });
    }
  },
  computed: {
    currentText() {
      return this.isEnglish ? this.englishText : this.latinText;
    }
  }
};
</script>

<style>
.book-container {
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 17vh;
}

.search-container {
    width: 100%;
    max-width: 4000px;
}

.results-wrapper {
  padding-bottom: 50px;
}

.image-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-top: 20px; 
}

.placeholder-container {
  width: 300px;
  margin: 20px auto;
  text-align: center;
}

.search-placeholder {
  font-family: 'Alegreya', sans-serif;
  font-size: 18px;
  white-space: nowrap;
  overflow: hidden;
  border-right: 2px solid;
  margin-top: 10px;
  padding-top: 10px;
  animation: typing 5s steps(40, end) infinite;
}

@keyframes typing {
  from {
    width: 0;
  }
  to {
    width: 100%;
  }
}

.hero {
  background-color: #ede8e563; /* Match this color with your footer */
}

.hero-title {
  font-size: 7rem;
  font-weight: bold;
  text-align: center;
  /* margin-top: 4rem; */
  color: #333; /* Adjust the color as needed */
  font-family: 'Alegreya', sans-serif;
}

.hero-desc {
  font-size: 2rem;
  text-align: center;
  color: #545252; /* Adjust the color as needed */
  font-family: 'Alegreya', sans-serif;
}

.hero-content {
  padding-top: 4rem;
  border-radius: 10px;
}

main {
  text-align: center;
  max-width: 1100px;
  align-items: center;
  margin: 0 auto;
  position: relative;
}

header {
  margin-bottom: 2rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.header-filters {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.sort-select {
  height: 40px;
  border: 1px solid #ffab2d;
  border-radius: 6px;
  color: #ffab2d;
  background: #fffbf4;
  font-weight: 600;
  cursor: pointer;
  transition: 0.3s;
  padding: 0 1rem;
}

.button-group {
  display: -webkit-box;
  max-width: 100%;
  overflow: auto;
}

.button-group button {
  border-radius: 6px;
  margin-right: 10px;
}


input {
  height: 50px;
  border-radius: 6px;
  border: 1px solid rgba(2, 28, 62, .1);
  font-size: 1rem;
  padding: 0 1rem;
}

button {
  font-size: 1rem;
  height: 40px;
  display: grid;
  place-items: center;
  margin-right: 0.4rem;
  padding: 0 1rem;
  border: 1px solid #ffab2d;
  border-radius: 6px;
  color: #ffab2d;
  background: #fffbf4;
  font-weight: 600;
  cursor: pointer;
  transition: 0.3s;
}

button.active {
  background: #ffab2d;
  color: #fffbf4;
}

h1 {
  font-weight: 700;
}

.movie-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 2rem 1rem;
}

@media screen and (max-width: 1024px) {
  .movie-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

.v-move,
.v-enter-active,
.v-leave-active {
  transition: 0.4s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 5;
  transform: translateY(10px);
}

.list-move,
.list-enter-active,
.list-leave-active {
  transition: 0.4s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateY(10px);
}

.list-leave-active {
  position: absolute;
  right: 0;
  left: 0;
}

.sort-select:focus {
  outline: none;
}

.sort-select option {
  background: #fffbf4;
  color: #ffab2d;
}
.sort-label {
  margin-right: 1rem;
}

.results-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  margin-top: 2rem;
}

.header-controls {
  display: flex;
  gap: 1rem;
}

.pagination-space {
  margin-top: 3rem;
}

.info-text {
  font-family: 'Alegreya', sans-serif; 
  color: #333;
  font-size: 16px; 
  line-height: 1.6;
  margin-bottom: 20px;
  text-align: center; 
  padding: 10px; 
  border-radius: 5px; 
  background-color: #f9f9f9; 
  box-shadow: 0 2px 4px rgba(0,0,0,0.1); 
  max-width: 80%; 
  margin-left: auto; 
  margin-right: auto;
}

.filter-button {
  margin-right: 10px;
}

</style>
