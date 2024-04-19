<template>
    <div class="modal" tabindex="-1" role="dialog" :class="{ 'show': showModal }" style="display: block;">
        <div class="modal-dialog modal-dialog-centered" style="max-width: 600px;">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">{{ book.volumeInfo.title }}</h5>
                    <button type="button" class="btn-close" aria-label="Close" @click="closeModal"></button>
                </div>
                <div class="modal-body">
                    <div class="scrollable-content">
                        <h2>Description: </h2>
                        <p v-if="!book.volumeInfo.description">No Details Found..</p>
                        <p v-else>{{ book.volumeInfo.description }}</p>
                    </div>
                </div>
                <div class="modal-footer">
                    <a v-if="book.saleInfo.buyLink" :href="book.saleInfo.buyLink" target="_blank">
                        <i class="fas fa-shopping-cart"></i> Buy Now
                    </a>
                    <a v-if="book.accessInfo.epub.downloadLink" :href="book.accessInfo.epub.downloadLink" target="_blank">
                        <i class="fas fa-download"></i> Download for Free (EPUB)
                    </a>
                    <a v-if="book.accessInfo.pdf.downloadLink" :href="book.accessInfo.pdf.downloadLink" target="_blank">
                        <i class="fas fa-download"></i> Download for Free (PDF)
                    </a>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script>
export default {
    props: {
        book: {
            type: Object,
            required: true
        },
        showModal: {
            type: Boolean,
            required: true
        }
    },
    methods: {
        closeModal() {
            this.$emit('close');
        }
    }
};
</script>
  
<style scoped>
.modal {
    background-color: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(5px);
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    z-index: 10000;
}

.modal-dialog {
    display: flex;
    align-items: center;
    justify-content: center;
}

.modal-content {
    max-width: 600px;
    height: 400px;
    display: flex;
    flex-direction: column;
}

.modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
    border-bottom: 1px solid #ccc;
    background-color: rgb(177 165 165 / 30%);
}

.modal-body {
    flex-grow: 1;
    overflow-y: auto;
    padding: 15px;
    background-color: lightyellow;
}

.modal-footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
    border-top: 1px solid #ccc;
    background-color: rgb(177 165 165 / 30%);
}

.modal-footer a {
    display: flex;
    align-items: center;
    text-decoration: none;
    color: black;
    cursor: pointer;
}

.modal-footer a i {
    font-size: 24px;
    margin-right: 10px;
}
</style>
