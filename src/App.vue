<template>
  <div id="container">
    <div class="row">
      <div class="col-md-8 offset-md-2 text-center">
        <h2 class="mt-5">ToDo List | Vue Js</h2>
        <hr />
        <div class="row">
          <div
            class="col-md-6 offset-md-3 pt-2 pb-2 d-flex flex-row align-items-center justify-content-between"
          >
            <input type="text" v-model="todoText" />
            <button @click="addTodo()" class="btn btn-primary">
              Ekle
            </button>
          </div>
        </div>
        <hr />
        <div class="todo-container">
          <Todo
            @deleteTodo="deleteTodo($event)"
            v-for="todo in todoList"
            :todo="todo"
            :key="todo.id"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Todo from "@/components/Todo";
import axios from "axios";
export default {
  components: {
    Todo
  },
  data() {
    return {
      todoText: "",
      todoList: []
    };
  },
  methods: {
    addTodo() {
      axios
        .post(
          "https://vuejs-todo-6802a-default-rtdb.firebaseio.com/todoList.json",
          {
            text: this.todoText
          }
        )
        .then(response => {
          console.log(response);
          this.todoList.push({
            id: response.data.name,
            text: this.todoText
          });
        })
        .catch(e => {
          console.log(e);
        });
    },
    deleteTodo(todoId) {
      axios
        .delete(
          "https://vuejs-todo-6802a-default-rtdb.firebaseio.com/todoList/" +
            todoId +
            ".json"
        )
        .then(() => {
          let index = this.todoList.findIndex(i => {
            return i.id == todoId;
          });

          this.todoList.splice(index, 1);
        })
        .catch(e => console.log(e));
    }
  },
  created() {
    axios
      .get("https://vuejs-todo-6802a-default-rtdb.firebaseio.com/todoList.json")
      .then(response => {
        for (let key in response.data) {
          //this.todoList.push(response.data[key]);
          let todo = {
            text: response.data[key].text,
            id: key
          };
          //console.log(response.data[key]);
          this.todoList.push(todo);
        }
      })
      .catch(e => console.log(e));
  }
};
</script>
