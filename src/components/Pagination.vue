<script setup>
import { defineEmits, defineProps } from "vue";

const emit = defineEmits({
  changePage: Number,
});

const { page, totalPage } = defineProps({
  page: Number,
  totalPage: Number,
});

function changePage(newPage) {
  emit("changePage", newPage);
}
</script>

<template>
  <div>
    <button
      class="pagination-button"
      id="prevPage"
      @click="changePage(page - 1)"
      v-show="page > 1"
    >
      &lt;
    </button>
  <span>Page {{ page }}/{{ totalPage }}</span>
    <button
      class="pagination-button"
      id="nextPage"
      @click="changePage(page + 1)"
      v-show="page < totalPage"
    >
      &gt;
    </button>
  </div>
</template>

<style scoped>
.pagination-button {
  background-color: #fff;
  color: #00b894;
  padding: 8px 12px;
  border: none;
  border-radius: 50%;
  cursor: pointer;
  font-size: 16px;
  margin: 0 5px;
}
</style>