<template>

<div id ="app" class= "container">
  <h2> Todo List</h2>
  <md-card>
  <md-header>
  <md-field>
  <md-input type ="text" v-model="currentTodo"  class ="todo-input" @keydown.enter="addTodo()" placeholder="Add a todo"></md-input>
  </md-field>
  <ul>
    <li v-for="todo in todos" :key="todo.id" >
      <div class ="todo-item-left">
        <md-checkbox v-model="todo.completed" class="md-primary" </md-checkbox>
          <div v-if= "!todo.editing"  @dblclick="editTodo(todo)" class ="todo-item-label" :class="{completed: todo.completed}">
            {{todo.label }}
          </div>
        <input v-else  class="todo-item-edit" type="text" v-model="todo.label"  @blur ="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
        </div>
      <div class ="close" @click="removeTodo(todo)">&times;</div>
    </li>
  </ul>
  </md-header>
  <md-container>
  <div class ="extra-container">
    <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">Check All</label></div>
    <div>{{remaining}} items left </div>
  </div>
  <div><small>Double click to edit a Todo</small></div>
  </md-container>
</md-card>
</div>

</template>

<script>
  export default {
    data() {
      return {
      todos: [],
      currentTodo: '',
      beforeEditCache:''
      };
    },

    computed: {
      remaining() {
        return this.todos.filter(todo =>!todo.completed).length;
        },

      anyRemaining() {
      return this.remaining !=0;
      }
    },

    directives: {
      focus: {
        inserted: function(el) {
        el.focus
        }
      }
    },

    methods: {
      addTodo() {
        if (this.currentTodo.trim().length==0){
        return
        }
        this.todos.push({id: this.todos.length, label: this.currentTodo, completed: false , editing: false});
        this.currentTodo = '';
      },

    editTodo(todo) {
      this.beforeEditCache=todo.label;
      todo.editing =true;
    },

    doneEdit(todo) {
      if (todo.label.trim()==''){
      todo.label=this.beforeEditCache;
      }
      todo.editing=false;
    },

    cancelEdit(todo) {
      todo.label= this.beforeEditCache
      todo.editing=false;
    },

    removeTodo(todo) {
      var index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
      },

    checkAllTodos() {
      this.todos.forEach((todo)=>todo.completed=event.target.checked);
      }
    }
  };
</script>


<style lang="scss">
  * {
    box-sizing:border-box;
    margin:0,auto;
    padding:auto;
    text-align:center;
    }
  .container {
    max-width:400px;
    margin:0,auto;
    text-align:center;
    padding-top:100px;
    }
  ul {
    margin: 0;
    padding: 0;
    }
  ul li {
    cursor: pointer;
    position: relative;
    padding: 12px 8px 12px 40px;
    list-style-type: none;
    font-size: 18px;
    transition: 0.2s;
  }

  #app {
    margin:0 auto;
    font-family:'Avenir', Arial, Sans-serif;
    color:#2c3e50;
    font-size:24px;
    }

  h2 {
    font-style:italic;
    font-size:50px;
  }

  .todo-input {
    width: 100%;
    padding: 10px 18px;
    font-size: 18px;
    margin-bottom:16px;
    }

  .todo-input:focus {
    outline:0;
    }

    .remove-item {
    padding-top :5px;
    align-items:center;
    justify-content:space-between;
    cursor:pointer;
    margin-left:20px;
    }

  .remove-item:hover {
    color:black;
    }

  .todo-item-left {
    display:flex;
    align-items:center;
    }

  .todo-item-label {
    border:1px solid white;
    margin-left: 12px;
    cursor:pointer;
    font-size:16px;
  }

  .todo-item-edit {
    color:#2c3e50;
    font-size:24px;
    margin-left:12px;
    width:200px;
    padding:10px;
    border:1px solid ;
    font-family:'Avenir', Arial, Sans-serif;
  }

  .completed {
    text-decoration:line-through;
    color:gray;
  }

  .extra-container {
  display:flex;
  align-item:center;
  justify-content:space-between;
  font-size:13px;
  border-top: 1px solid gray;
  padding-top:14px;
  margin-bottom:14px;
  }

  small {
    font-size:13px;
  }
  .close {
    position: absolute;
    right: 0;
    top: 0;
    padding: 12px 16px 12px 16px;
  }

</style>
