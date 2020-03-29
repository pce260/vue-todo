<template>
  <div class="hello">
    <h1>ToDo Application</h1>
    <div>
      <input type="text" v-model="newContent" @keyup.enter="addToDo()" placeholder="Input ToDo" />
      <button @click="addToDo">Add</button>
    </div>
    <ul v-for="todo in todos" :key="todo.id">
      <li>
        <div>
          <label>
            <input type="checkbox" />{{todo.content}}
          </label>
          <button @click="deleteToDo(todo.id)">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'ToDo',
  data()  {
    return {
      todos: [],
      newContent: '',
      currentId: 0
    }
  },
  methods: { 
    addToDo() {
      if (this.newContent) {
        this.todos.push({
          id: this.currentId,
          content: this.newContent,
          status: 'in progress'
        });
        this.newContent = '';
        this.currentId++;
      }
    },
    deleteToDo(id) {
      this.todos = this.todos.filter(function (todo) {
        return todo.id !== id;
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
