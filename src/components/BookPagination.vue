<template>
  <nav aria-label="Page navigation">
    <ul class="pagination">
      <li class="page-item" :class="{ disabled: currentPage === 1 }">
        <button class="page-link" @click="goToFirstPage" :disabled="currentPage === 1">First</button>
      </li>
      <li class="page-item" :class="{ disabled: currentPage === 1 }">
        <button class="page-link" @click="changePage(currentPage - 1)" :disabled="currentPage === 1">Previous</button>
      </li>
      <li v-for="page in displayedPages" :key="page" class="page-item" :class="{ active: currentPage === page }">
        <button class="page-link" @click="changePage(page)">{{ page }}</button>
      </li>
      <li v-if="displayedPages[displayedPages.length - 1] < totalPages">
        <button class="page-link" @click="incrementDisplayedPages">...</button>
      </li>
      <li class="page-item" :class="{ disabled: currentPage === totalPages }">
        <button class="page-link" @click="changePage(currentPage + 1)" :disabled="currentPage === totalPages">Next</button>
      </li>
      <li class="page-item" :class="{ disabled: currentPage === totalPages }">
        <button class="page-link" @click="goToLastPage" :disabled="currentPage === totalPages">Last</button>
      </li>
    </ul>
    <div class="go-to-page">
      <input type="number" min="1" :max="totalPages" v-model.number="inputPage" @keydown.enter="goToPage">
      <button @click="goToPage" class="btn btn-sm btn-primary">Go</button>
    </div>
  </nav>
</template>

<script>
export default {
  props: {
    total: {
      type: Number,
      required: true,
    },
    perPage: {
      type: Number,
      default: 10,
    },
    currentPage: {
      type: Number,
      default: 1,
    },
  },
  data() {
    return {
      inputPage: 1,
      maxDisplayedPages: 5, // Define the maximum number of displayed pages
    };
  },
  computed: {
    totalPages() {
      return Math.ceil(this.total / this.perPage);
    },
    // Calculate the displayed pages based on the current page and the maximum displayed pages
    displayedPages() {
      const start = Math.max(1, this.currentPage - Math.floor(this.maxDisplayedPages / 2));
      const end = Math.min(this.totalPages, start + this.maxDisplayedPages - 1);
      const pages = [];
      for (let i = start; i <= end; i++) {
        pages.push(i);
      }
      return pages;
    },
  },
  methods: {
    changePage(pageNumber) {
      if (pageNumber > 0 && pageNumber <= this.totalPages) {
        this.$emit('page-change', pageNumber);
      }
    },
    goToPage() {
      const pageNumber = parseInt(this.inputPage);
      if (pageNumber > 0 && pageNumber <= this.totalPages) {
        this.changePage(pageNumber);
      }
    },
    goToFirstPage() {
      this.changePage(1);
    },
    goToLastPage() {
      this.changePage(this.totalPages);
    },
    // Method to increment the displayed pages
    incrementDisplayedPages() {
      // Move the displayed pages forward by the maximum number of displayed pages
      const newStart = this.displayedPages[0] + this.maxDisplayedPages;
      this.changePage(newStart);
    },
  },
};
</script>

<style scoped>
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
}

.page-item {
  cursor: pointer;
  margin: 0 5px;
}

.page-link {
  border: none;
  background: none;
  outline: none;
}

.active .page-link {
  background-color: #007bff; 
  color: white;
  border-radius: 5px;
  padding: 5px 10px; 
}

.disabled .page-link {
  pointer-events: none;
  opacity: 0.5;
}

.go-to-page {
  margin-top: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.go-to-page input[type="number"] {
  height: 30px;
  padding: 5px;
}

.go-to-page button {
  height: 30px;
  margin-left: 5px;
}
</style>
