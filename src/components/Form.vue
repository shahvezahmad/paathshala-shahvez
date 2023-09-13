<template>
    <form @submit.prevent="submitForm" class="custom-form">
        <h2 class="form-title">Apply Now</h2>

        <div class="form-group">
        <label for="name" class="form-label">Full Name:</label>
        <input type="text" name="name" id="name" required v-model="inputData.name" class="form-input" />
        </div>

        <div class="form-group">
        <label for="email" class="form-label">Email:</label>
        <input type="email" name="email" id="email" required v-model="inputData.email" class="form-input" />
        </div>

        <div class="form-group">
        <label for="phone" class="form-label">Phone Number:</label>
        <input type="text" name="phone" id="phone" required v-model="inputData.phone" class="form-input" />
        </div>

        <div class="form-group">
        <label for="role" class="form-label">Role:</label>
        <select name="role" id="role" v-model="inputData.role" class="form-input">
            <option value="Frontend-developer">Frontend Developer</option>
            <option value="Backend-developer">Backend Developer</option>
            <option value="Fullstack-developer">Full stack Developer</option>
        </select>
        </div>

        <div class="form-group">
        <label for="skills" class="form-label">Skills:</label>
        <input type="text" name="skills" id="skills" required v-model="inputData.skills" class="form-input" />
        </div>

        <button type="submit" class="form-button">Apply</button>
    </form>
</template>
  

<script>
    import { API } from "aws-amplify";
    import { createTodo } from "../graphql/mutations";
    import { listTodos } from "../graphql/queries";

    export default {
        data() {
            return {
                inputData: {
                    name: '',
                    email: '',
                    phone: '',
                    role: 'Frontend-developer',
                    skills: '',
                },
                todos: [],
            };
        },
        mounted() {
            this.focusInput();
        },
        methods: {
            async submitForm() {
                const { name, email, phone, role, skills } = this.inputData;
                const todo = { name, email, phone, role, skills };
                console.log(todo);
                try {
                    await API.graphql({
                    query: createTodo,
                    variables: { input: todo },
                    });
                } catch (error) {
                    console.log(error);
                }
                this.inputData.name = "";
                this.inputData.email = "";
                this.inputData.phone = "";
                this.inputData.role = "frontend-developer";
                this.inputData.skills = "";
            },

            async getTodos() {
                try{
                    const response = await API.graphql({
                    query: listTodos,
                }   );
                    const getData = response.data.listTodos.items;
                    console.log(getData);
                } catch (error) {
                    console.error('Error fetching data:', error);
                }   
            },
            focusInput() {
                this.$nextTick(() => {
                    if (this.$refs.nameField) {
                        this.$refs.nameField.focus();
                    }
                });
            },

        },
    };
</script>

<style scoped>
    .custom-form {
        max-width: 400px;
        margin: 0 auto;
        padding: 20px;
        background-color: #ffffff;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        border-radius: 10px;
        text-align: center;
    }

    .form-title {
        font-size: 24px;
        margin-bottom: 20px;
        color: #333;
    }

    .form-group {
        margin-bottom: 20px;
    }

    .form-label {
        display: block;
        font-size: 16px;
        font-weight: bold;
        color: #555;
        margin-bottom: 6px;
    }

    .form-input {
        width: 100%;
        padding: 10px;
        font-size: 16px;
        border: 1px solid #ccc;
        border-radius: 4px;
        outline: none;
        transition: border-color 0.3s;
    }

    .form-input:focus {
        border-color: #1bad08;
    }

    .form-button {
        background-color: #1bad08;
        color: #fff;
        border: none;
        padding: 12px 20px;
        font-size: 18px;
        border-radius: 5px;
        cursor: pointer;
        transition: background-color 0.3s;
    }

    .form-button:hover {
        background-color: #159c06;
    }

</style>
  