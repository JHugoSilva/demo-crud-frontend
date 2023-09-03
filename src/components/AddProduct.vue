<template>
    <form @submit.prevent="submitForm" class="add-product-form">
        <input class="input-field" v-model="name" placeholder="Product Name">
        <p class="error" v-if="errors.name">{{ errors.name }}</p>
        <input class="input-field" v-model="description" placeholder="Product Description">
        <p class="error" v-if="errors.description">{{ errors.description }}</p>
        <input class="input-field" v-model="price" placeholder="Product Price">
        <p class="error" v-if="errors.price">{{ errors.price }}</p>
        <button type="submit" class="submit-button">Add Product</button>
    </form>
</template>
<script>
import axios from '@/axios';

export default {
    data() {
        return {
            name: '',
            description: '',
            price: '',
            errors: {}
        }
    },
    methods: {
        validateInput() {
            const errors = {}
            if (!this.name) errors.name = 'Name is required'
            if (!this.description) errors.description = 'Description is required'
            if (!this.price || isNaN(this.price)) errors.price = 'Price is required'
            return errors
        },
        async submitForm() {
            const errors = this.validateInput()
            if (Object.keys(errors).length > 0) {
                this.errors = errors
                return 
            }
            try {
                await axios.post('products',{
                    name: this.name,
                    description: this.description,
                    price: this.price
                })
                this.$router.push('/')
            } catch (error) {
                console.log('An error currred while adding the product', error)
            }
        }
    }
}
</script>
<style scoped>
.error {
    color: red;
}
.add-product-form {
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