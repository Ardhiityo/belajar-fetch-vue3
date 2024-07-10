<script setup>
import ProductCard from "@/components/ProductCard.vue";
import Pagination from "@/components/Pagination.vue";
import Loading from "@/components/Loading.vue";
import ProductForm from "@/components/ProductForm.vue";

import { ref, watchEffect } from "vue";
import axios from "axios";

const products = ref([]);

const page = ref(1);
const limit = ref(8);
const loading = ref(true);

async function fectProducts() {
  const api = `http://localhost:3000/products?_page=${page.value}&_per_page=${limit.value}`;
  try {
    loading.value = false;
    const res = await axios.get(api);
    products.value = res.data;
  } catch (error) {
    console.log(error);
  } finally {
    loading.value = false;
  }
}

watchEffect(() => fectProducts());

function changePage(newPage) {
  if (newPage < 1) return;
  if (newPage > products.value.pages) return;
  return (page.value = newPage);
}

async function addProduct(data) {
  try {
    await axios.post("http://localhost:3000/products", data);
    fectProducts();
  } catch (error) {
    console.log(error);
  }
}
</script>

<template>
  <div v-if="loading">
    <Loading />
  </div>
  <main v-else>
    <ProductForm @add-product="addProduct" />
    <div class="product-grid">
      <ProductCard
        v-for="product in products.data"
        :key="product.id"
        :product="product"
      />
    </div>
    <div class="pagination">
      <Pagination
        :page="page"
        :totalPage="products.pages"
        @changePage="changePage"
      />
    </div>
  </main>
</template>

<style scoped>
.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
  width: 80%;
  margin: 0 auto;
}

.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}
</style>