<template>
  <div id="app">
    <div class="list-todos">
      <div class='input-todo'>
        <input type="text" placeholder="Input todo" ref="todo" @keyup.enter='addTodo'/>
        <button class="btn-success" @click="addTodo">Add todo</button>
      </div>
      <h2>LIST TODOS</h2>
      <ItemTodo
        v-for="(item, index) in listTodos"
        :key="item.id"
        :singleTodo="item"
        :index="index"
        @increase="increaseDone"
        @decrease="decreaseDone"
        @deleteItem="deleteItem"
        @updateList="updateList"
      />
      Tổng số công việc đã hoàn thành: {{ checkItems }}
    </div>
  </div>
</template>

<script>
import ItemTodo from "./components/ItemTodo";

export default {
  name: "App",
  components: {
    ItemTodo,
  },
  data() {
    return {
      listTodos: [],
      checkItems: 0,
    };
  },
  mounted() {
    const data = localStorage.getItem("list-todos");
    if (data){
      this.listTodos = JSON.parse(data);
      const arr = this.listTodos.filter(item => item.checked === true)
      this.checkItems = arr.length
    }
  },
  methods: {
    getKey() {
      return new Date().getTime();
    },
    saveToLocalStore() {
      localStorage.setItem("list-todos",JSON.stringify(this.listTodos))
    },
    addTodo() {
      const value = this.$refs.todo.value;
      this.$refs.todo.focus();
      if (!value) return;
      this.listTodos.push({ content: value, id: this.getKey(), checked: false });
      this.saveToLocalStore()
      this.$refs.todo.value = "";
    },
    increaseDone(index) {
      this.checkItems += 1;
      this.listTodos[index].checked = true;
      this.saveToLocalStore()
    },
    decreaseDone(index) {
      this.checkItems -= 1;
      if (this.listTodos[index]) 
        this.listTodos[index].checked = false;
      this.saveToLocalStore()

    },
    deleteItem(index) {
      if (this.listTodos[index].checked) this.decreaseDone();
      this.listTodos.splice(index, 1);
      this.saveToLocalStore()
    },
    updateList(index, content){
      this.listTodos[index].content = content
      localStorage.setItem("list-todos",JSON.stringify(this.listTodos))
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

input[type="text"] {
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  line-height: 1.5;
  color: #495057;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}
.list-todos {
  width: 500px;
  margin: 0 auto;
}

.btn-success {
  color: #fff;
  background-color: #28a745;
  border-color: #28a745;
}

.btn-primary {
  color: #fff;
  background-color: #3c71e4;
  border-color: #3c71e4;
}

.btn-danger {
  color: #fff;
  background-color: #f03453;
  border-color: #f03453;
}

.input-todo {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

button {
  cursor: pointer;
  display: inline-block;
  font-weight: 400;
  text-align: center;
  white-space: nowrap;
  vertical-align: middle;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  border: 1px solid transparent;
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  line-height: 1.5;
  border-radius: 0.25rem;
  transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out,
    border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}
</style>
