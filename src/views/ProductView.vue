<script setup>
import { useRoute, useRouter } from "vue-router";
import { ref } from "vue";
import axios from "axios";

const route = useRoute();
const router = useRouter();
import ProductForm from "@/components/ProductForm.vue";

const product = ref({});
const api = `http://localhost:3000/products/${route.params.id}`;

async function getProduct() {
  const res = await axios.get(api);
  return (product.value = res.data);
}

try {
  getProduct();
} catch (error) {
  console.log(error);
}

async function deleteProduct() {
  try {
    await axios.delete(api);
    router.push("/");
  } catch (error) {
    console.log(error);
  }
}

async function updateProduct(product) {
  try {
    await axios.put(api, product);
    router.push("/");
  } catch (error) {
    console.log(error);
  }
}
</script>

<template>
  <div class="product-detail">
    {{ product.title }}
    <h2>{{ product.title }}</h2>
    <img :src="product.image" :alt="product.title" class="product-image" />
    <p>Description: {{ product.description }}</p>
    <p>Price: {{ product.price }}</p>
    <ProductForm :product="product" @update-product="updateProduct" />
    <router-link to="/">
      <button class="back-button">Back</button>
    </router-link>
    <button class="delete-button" @click="deleteProduct">Delete</button>
  </div>
</template>

<style scoped>
.product-detail {
  margin-top: 20px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
  text-align: center;
}
.product-image {
  max-width: 100%;
  height: auto;
  margin-bottom: 10px;
}
.product-detail p {
  margin-bottom: 5px;
}
.product-detail button {
  margin-top: 10px;
  padding: 10px 20px;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
.back-button {
  display: inline-block;
  padding: 8px 16px;
  background-color: #007bff;
  color: #fff;
  text-decoration: none;
  border-radius: 4px;
  transition: background-color 0.3s;
}
.back-button:hover {
  background-color: #0056b3;
}
.delete-button {
  display: inline-block;
  padding: 8px 16px;
  background-color: #dc3545;
  color: #fff;
  text-decoration: none;
  border-radius: 4px;
  transition: background-color 0.3s;
}
.delete:hover {
  background-color: #bd2130;
}
</style>