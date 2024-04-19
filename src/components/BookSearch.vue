<template>
  <div class="search-container">
    <div class="search-wrapper">
      <div class="input-group mb-3">
        <input type="text" class="form-control search-input" :style="{ borderColor: invalidSearch ? 'red' : '#ccc' }" 
          placeholder="Search Books" v-model="searchQuery" @keyup.enter="searchBooks" @focus="onFocus" @blur="onBlur">
        <button class="btn btn-primary search-btn" type="button" @click="searchBooks">
          <i class="fas fa-search"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: '',
      isFocused: false,
      invalidSearch: false,
    };
  },
  methods: {
    searchBooks() {
      if(this.searchQuery) {
        this.invalidSearch = false
        this.$emit('search', this.searchQuery);
      } else {
        this.invalidSearch = true;
      }
    },
    onFocus() {
      this.isFocused = true;
    },
    onBlur() {
      this.isFocused = false;
    }
  }
};
</script>

<style scoped>
.search-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 10vh;
}

.search-wrapper {
  width: 50%;
}

.search-input {
  border-radius: 20px;
  border-width: 2px;
  border-style: solid;
  padding: 10px;
  transition: border-color 0.3s;
  width: calc(100% - 60px);
  height: 40px;
}

.search-input:focus {
  border-color: #007bff;
}

.search-btn {
  border-radius: 20px;
  border: 2px solid #007bff;
  background-color: #007bff;
  color: #fff;
  transition: background-color 0.3s, color 0.3s;
  width: 40px;
  width: 60px;
  height: 40px;
  margin-left: -2px;
}

.search-btn:hover {
  background-color: #0056b3;
}

.search-btn:focus {
  outline: none;
}

.fa-search {
  font-size: 18px;
}
</style>
