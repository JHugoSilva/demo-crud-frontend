<template>
    <div class="form-container">
        <form @submit.prevent="register" class="login-form">
            <input type="text" v-model="name" placeholder="Name" required/>
            <input type="email" v-model="email" placeholder="Email" required/>
            <input type="password" v-model="password" placeholder="Password" required/>
            <button type="submit">Register</button>
        </form>
    </div>
</template>

<script>
import axios from "@/axios";

export default {
    data() {
        return {
            name: "",
            email: "",
            password: ""
        }
    },
    methods: {
        async register() {
            try {
                const response = await axios.post("register",{
                    name: this.name,
                    email: this.email,
                    password: this.password
                })
            } catch (error) {
                console.error('An error occurred:', error)
                if (error.response) {
                    console.error("Error details:", error.response.data)
                }
            }
        }
    }
}

</script>
<style scoped>
    .form-container {
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .login-form {
        display: flex;
        flex-direction: column;
    }

    .login-form {
        display: flex;
        flex-direction: column;
        gap: 20px;
        width: 300px;
        padding: 20px;
        border-radius: 5px;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    .login-form input, .login-form button {
        padding: 10px;
        border-radius: 5px;
        border: 1px solid #CCC;
        font-size: 16px;
    }

    .login-form button {
        background-color: #007BFF;
        color: white;
        cursor: pointer;
    }
</style>