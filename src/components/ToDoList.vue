 <template>
  <div>
     <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    <div v-for="(todo, index) in todosFiltred" :key="todo.id" class="todo-item">
      <div class="todo-item-left">
        <input type="checkbox" v-model="todo.completed">
        <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{ completed : todo.completed }">{{todo.title}}</div>
        <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
      </div>
      <div class="remove-item" @click="removeTodo(index)">
        &times;
      </div>
    </div>
    <div class="extra-container">
      <div><label><input type="checkbox" :checked="!anyRemainning" @change="checkAllTodos">Check All</label></div>
      <div> {{ remainning }} items left</div>
    </div>

   <div class="extra-container">
     <div>
       <button :class="{ active: filter == 'all' }" @click="filter= 'all'">All</button>
       <button :class="{ active: filter == 'all' }" @click="filter= 'active'">active</button>
       <button :class="{ active: filter == 'all' }" @click="filter= 'Completed'">Completed</button>
     </div>

   <div>
     <transition name="fade">
     <button v-if="showClearCompletedButton" @click="clearCompleted">Clear completed</button>
     </transition>
   </div>
   </div>

  </div>
</template>

<script>
export default {
  name: 'ToDo-list',
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
      beforeEditCache: '',
      filter: 'all',
      todos: [
        {
          'id': 1,
          'title': 'Finish Vue Screencast',
          'completed': false,
          'editing' : false,
        },
        {
          'id': 2,
          'title': 'Take over Word',
          'completed': false,
          'editing' : false,
        },
      ]
    }
  },
  computed: {
    remainning() {
      return this.todos.filter(todo => !todo.completed).length
    },

    anyRemainning() {
      return this.remainning !== 0
  },
    todosFiltred() {
      if (this.filter == 'all') {
        return this.todos
      } else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }
       return this.todos
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0
    }
  },

  directive: {
  focus:{
    inserted:function (el) {
      el.focus()
    }
  }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        return
      }

      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
      })

      this.newTodo = ''
      this.idForTodo++
     },

    editTodo(todo) {
      this.beforeEditCache = todo.title
      todo.editing = true
    },

    doneEdit(todo) {
      if (todo.title.trim() == 0) {
        todo.title =this.beforeEditCache
      }
      todo.editing = false
    },

    cancelEdit(todo) {
      todo.title = this.beforeEditCache
      todo.editing = false
    },

     removeTodo(index) {
      this.todos.splice(index, 1)
     },

    checkAllTodos() {
      this.todos.forEach((todo) => todo.completed = event.target.checked)
    },

    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
    }
}
</script>

<style lang="scss">
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;

  &:focus {
    outline: 0;
  }
}

.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.remove-item {
  cursor: pointer;
  margin-left: 14px;
  &:hover {
    color: black;
  }
}

.todo-item-left {
  display: flex;
  align-items: center;
}

.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}

.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;

  &:focus {
    outline: none;
  }
}

.completed {
  text-decoration: line-through;
  color: grey;
}

.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgray;
  padding-top: 14px;
  margin-bottom: 14px;
}
button {
  font-size: 14px;
  background-color: white;
  appearance: none;

  &:hover {
    background: lightgreen;
  }
  &:focus {
    outline: none;
  }
  .active {
    background: lightgreen;
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity .2s;
  }
  fade-enter, fade-leave-to {
    opacity: 0;
  }
}
</style>
