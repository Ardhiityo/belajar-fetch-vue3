<script setup>
import { ref, defineProps, watchEffect, computed } from "vue";
const emit = defineEmits(["add-product", "update-product"]);

const props = defineProps(["product"]);

const showForm = ref(false);

const title = ref("");
const description = ref("");
const price = ref("");
const image = ref("");
const id = ref("");

watchEffect(() => {
  title.value = props.product?.title;
  description.value = props.product?.description;
  price.value = props.product?.price;
  image.value = props.product?.image;
  image.value = props.product?.image;
  id.value = props.product?.id;
});

const statusForm = computed(() => (id.value ? true : false));

function showAddForm() {
  showForm.value = true;
}

function closeForm() {
  showForm.value = false;
}

function addProduct() {
  const product = {
    title: title.value,
    description: description.value,
    price: price.value,
    image: image.value,
  };

  if (statusForm.value) {
    emit("update-product", product);
  } else {
    emit("add-product", product);
  }

  return (showForm.value = false);
}
</script>
<template>
  <div class="add-product">
    <button @click="showAddForm">
      {{ statusForm ? "Edit" : "Add" }} Product
    </button>
    <div v-if="showForm" class="product-form">
      <form @submit.prevent="addProduct">
        <label for="title">Title:</label>
        <input type="text" id="title" v-model="title" required />
        <label for="description">Description:</label>
        <input type="text" id="description" v-model="description" required />
        <label for="price">Price:</label>
        <input type="number" id="price" v-model="price" required />
        <label for="image">Image:</label>
        <input type="text" id="image" v-model="image" required />
        <button type="submit">Add</button>
        <button type="button" @click="closeForm">Close</button>
      </form>
    </div>
  </div>
</template>
<style scoped>
.add-product {
  text-align: center;
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
}
.add-product button {
  padding: 10px 20px;
  margin-top: 10px;
  background-color: #28a745;
  color: #fff;
  border: none;
  border-radius: 5px;
}
.product-form {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 35%;
  height: 280px;
  background-color: whitesmoke;
  box-sizing: border-box;
  border: 1px solid black;
}
form {
  display: flex;
  justify-content: center;
  flex-direction: column;
  width: 90%;
  height: 85%;
  text-align: start;
}
form input {
  padding: 4px;
  border-radius: 5px;
}
</style>