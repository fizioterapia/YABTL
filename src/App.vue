<template>
  <div class="logo">
    <h1>YABTL</h1>
    <p>Yet Another Boring Todo List</p>
  </div>
  <div class="add">
    <input
      @keyup.enter="addTodo"
      placeholder="Enter todo text..."
      v-model="todoText"
      type="text"
    />
    <button class="icon" @click="addTodo"><i class="fas fa-plus"></i></button>
  </div>
  <ul>
    <li v-for="(elem, index) in todos" :key="index">
      <TodoComponent
        @modified="modifyTodo"
        :id="index"
        :todoText="elem.text"
        :priorities="priorities"
        :class="[elem.priority, 'component']"
      ></TodoComponent>
    </li>
  </ul>
</template>

<script>
import TodoComponent from "./components/TodoComponent.vue";

export default {
  name: "App",
  data() {
    return {
      todoText: "",
      todos: [],
      priorities: ["ASAP", "High", "Medium", "Low"],
    };
  },
  methods: {
    modifyTodo(type, id, val) {
      switch (type) {
        case "delete":
          this.todos = this.todos.filter((elem, index) => {
            return id != index;
          });
          break;
        case "moveUp":
          if (id > 0) {
            let tmp = this.todos[id];
            this.todos[id] = this.todos[id - 1];
            this.todos[id - 1] = tmp;
          }
          break;
        case "moveDown":
          if (id < this.todos.length) {
            let tmp = this.todos[id];
            this.todos[id] = this.todos[id + 1];
            this.todos[id + 1] = tmp;
          }
          break;
        case "priority":
          this.todos[id].priority = this.priorities[val].toLowerCase();
          break;
        default:
          break;
      }
    },
    addTodo() {
      if (this.todoText.length <= 0) return;

      this.todos.push({
        text: this.todoText,
        priority: "low",
      });

      this.todoText = "";
    },
  },
  components: {
    TodoComponent,
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;700&display=swap");

body,
html {
  width: 100%;
  height: 100%;
  margin: 0;

  color: white;

  background: linear-gradient(rgba(0, 0, 0, 0.75), rgba(0, 0, 0, 0.75)),
    linear-gradient(45deg, rgba(154, 59, 218, 0.75), rgba(59, 115, 218, 0.75)),
    url("assets/bg.jpg");
  font-family: "Inter", sans-serif;
}

.logo {
  text-align: center;
  margin: 32px;

  border-radius: 8px;

  h1,
  p {
    margin: 0px;
  }

  h1 {
    font-size: 3em;
  }

  p {
    opacity: 0.75;
  }
}

div.add {
  display: flex;
  align-items: center;

  background: #f0f0f0;
  border-radius: 4px;
  color: #161616;
  width: 25%;

  input {
    padding-left: 16px;
    width: 100%;
    height: 3em;
  }

  button {
    font-weight: 600;
  }
}

input,
button {
  border: none;
  background: none;
  color: inherit;
  outline: none;
}

button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  background: #f0f0f0;
  padding: 12px 24px;
  border-radius: 4px;
  color: #161616;
  margin: 4px;
}

button.icon {
  padding: 6px;
  font-size: 1em;
  width: 2em;
  height: 2em;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;

  li {
    margin: 16px 0;
  }
}

button:hover {
  filter: brightness(0.8);
}

ul {
  width: 25%;
}

.component {
  width: 100%;

  box-sizing: border-box;

  display: flex;
  justify-content: space-between;
  align-items: center;
}

li:first-child > .component .buttons .move-up {
  opacity: 0.25;
  pointer-events: none;
}

li:last-child > .component .buttons .move-down {
  opacity: 0.5;
  pointer-events: none;
}

#app {
  width: 100%;
  height: 100%;

  margin: 0px;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
