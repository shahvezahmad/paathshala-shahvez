<template>
  <h2 class="title">Applications</h2>
  <div class="main">
  <div class="card" v-for="item in todos" :key="item.id">
      <div class="card-header">
          <button class="close-button" @click="deleteItem(item.id)">X</button>
      </div>
      <div class="card-body">
      <h5 class="card-title">{{ item.name }}</h5>
      <p class="card-text"><span>Email: </span>{{ item.email }}</p>
      <p class="card-text"><span>Role: </span>{{ item.role }}</p>
      <p class="card-text"><span>Skills: </span>{{ item.skills }}</p>
      </div>
  </div>
  </div>
</template>

<script>
  import { API } from "aws-amplify";
  import { listTodos } from "../graphql/queries";
  import { onCreateTodo } from "../graphql/subscriptions";
  import { deleteTodo } from "../graphql/mutations";

  export default {
  data() {
      return {
      todos: [],
      };
  },
  async created() {
      this.getTodos();
      this.subscribe();
  },
  methods: {
      async getTodos() {
      const todos = await API.graphql({
          query: listTodos,
      });
      this.todos = todos.data.listTodos.items;
      },
      subscribe() {
      API.graphql({ query: onCreateTodo }).subscribe({
          next: (eventData) => {
          console.log('subscription running', eventData);
          let todo = eventData.value.data.onCreateTodo;
          console.log('Received todo:', todo);
          if (this.todos.some((item) => item.email === todo.email)){
              console.log('Error: you already submitted the form');
              return;
          }
          this.todos = [...this.todos, todo];
          },
      });
      },

      async deleteItem(itemId) {
      try {
        await API.graphql({
          query: deleteTodo,
          variables: {
            input: {
              id: itemId,
            },
          },
        });

        this.todos = this.todos.filter((item) => item.id !== itemId);
      } catch (error) {
        console.error('Error during application deletion: ', error);
      }
      },
  },
  };
</script>

<style scoped>
  .main {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }

  .title {
    font-size: 24px;
    margin-bottom: 20px;
    color: #333;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .card {
    width: 18rem;
    border: 1px solid #ccc;
    border-radius: 5px;
    overflow: hidden;
    margin: 20px;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.1);
    position: relative; 
  }

  .card-body {
    padding: 2rem;
    display: flex;
    flex-direction: column;
  }

  .card-title {
    font-size: 1.25rem;
    margin-bottom: 0.5rem;
  }

  .card-text {
    color: #666;
    margin-bottom: 0rem;
  }

  .close-button {
    position: absolute;
    top: 0;
    right: 0;
    margin: 0;
    padding: 5px; 
    background-color: red;
    color: white;
    border: none;
    border-radius: 0; 
    width: 24px;
    height: 24px;
    font-size: 16px;
    text-align: center;
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
  }



  @media (max-width: 768px) {
    .card {
      width: 100%;
      margin: 10px 0; 
    }
  }
</style>