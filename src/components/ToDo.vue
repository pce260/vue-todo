<template>
  <div class="hello">
    <h1>ToDo Application</h1>
    <div>
      <input type="text" v-model="newContent" @keyup.enter="addToDo()" placeholder="Input ToDo">
      <button @click="addToDo">Add</button>
    </div>
    <ul v-for="todo in todos" :key="todo.id">
      <li>
        <div>
          <input type="checkbox" v-model="todo.status" @change="updateToDo()">
          <input type="text" v-model="todo.content" @change="updateToDo()">
          <button @click="deleteToDo(todo.id)">Delete</button>
          <span v-show="todo.status">完了</span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
const STORAGE_KEY = 'todos';
export default {
  name: 'ToDo',
  data()  {
    return {
      todos: JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]'),
      newContent: ''
    }
  },
  methods: {
    addToDo() {
      if (this.newContent) {
        this.todos.push({
          id: this.generateUuid(),
          content: this.newContent,
          status: false
        });
        this.updateLocalStorage();
        this.newContent = '';
      }
    },
    updateToDo() {
      this.updateLocalStorage();
    },
    deleteToDo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
      this.updateLocalStorage();
    },
    updateLocalStorage() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos));
    },
    generateUuid() {
      // https://github.com/GoogleChrome/chrome-platform-analytics/blob/master/src/internal/identifier.js
      // const FORMAT: string = "xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx";
      let chars = "xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx".split("");
      for (let i = 0, len = chars.length; i < len; i++) {
        switch (chars[i]) {
          case "x":
            chars[i] = Math.floor(Math.random() * 16).toString(16);
            break;
          case "y":
            chars[i] = (Math.floor(Math.random() * 4) + 8).toString(16);
            break;
        }
      }
      return chars.join("");
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
