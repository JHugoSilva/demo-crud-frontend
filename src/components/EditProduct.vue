<template>
  <div class="edit-product">
    <form @submit.prevent="submitForm" class="edit-product-form">
      <input
        class="input-field"
        type="text"
        v-model.lazy="product.name"
        placeholder="Product Name"
      />
      <p v-if="errors.name" class="error">{{ errors.name }}</p>
      <input
        class="input-field"
        type="text"
        v-model="product.description"
        placeholder="Product Description"
      />
      <p v-if="errors.description" class="error">{{ errors.description }}</p>
      <input
        class="input-field"
        type="text"
        v-model="product.price"
        placeholder="Product Price"
      />
      <p v-if="errors.price" class="error">{{ errors.price }}</p>
      <button type="submit" class="submto-button">Update Product</button>
    </form>
  </div>
</template>
<script setup>
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";
import axios from "@/axios";

const props = defineProps({
  id: { type: String, default: "" },
});

const router = useRouter()

const product = ref({ id: "", name: "", description: "", price: "" });
const errors = ref({});

const validateInput = () => {
  const errorMessages = {};

  if (!product.value.name) errorMessages.name = "Name is required";
  if (!product.value.description)
    errorMessages.description = "Description is required";
  if (!product.value.price) errorMessages.price = "Price is required";
  return errorMessages;
};

const submitForm = async () => {
  const errorMessages = validateInput();
  if (Object.keys(errorMessages).length > 0) {
    errors.value = errorMessages;
    return;
  }

  try {
    await axios.put(`/products/${props.id}`, product.value);
    router.push("/");
  } catch (error) {
    console.log("An error ocurred while update the product:", error);
    if (error.response && error.response.status === 422) {
      error.value = error.response.data.errors;
    }
  }
};

onMounted(async () => {
  try {
    const response = await axios.get(`/products/${props.id}`);
    console.log(product);

    product.value.name = response.data.name;
    product.value.description = response.data.description;
    product.value.price = response.data.price;
  } catch (error) {
    console.error("An error ocurred while fetching the product: ", error);
  }
});
</script>
<style scoped>
.error {
    color: red;
}
.edit-product {
    max-width: 400px;
    margin: 20px auto;
    padding: 20px;
    background-color: #F9F9F9;
    border-radius: 8px;
}
.input-field {
    display: block;
    width: 100%;
    margin: 10px 0;
    padding: 10px;
    font-size: 1em;
}
.submit-button {
    padding: 10px;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}
.submit-button:hover {
    background-color: #45A049;
}
</style>