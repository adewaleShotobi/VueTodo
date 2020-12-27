<template>
  <div id="home">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Todos from '../components/Todos';
import AddTodo from '../components/layout/AddTodo';
export default {
  name: 'Home',
  components: {
    Todos, AddTodo
  },
  data(){
    return {
      todos : []
    }
  },
  methods : {
    deleteTodo(id){
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    addTodo(newTodo){
      const {title, completed} = newTodo;
      console.log(newTodo)
      fetch("http://jsonplaceholder.typicode.com/todos",{
        method : "POST",
        body: JSON.stringify({
          title,
          completed
        })
      })
      .then(res => res.json())
      .then(data => {
        newTodo.id = data.id;
         this.todos = [...this.todos,newTodo]
      })
      .catch(e => console.log(e))
    }
  },
  created(){
      fetch("http://jsonplaceholder.typicode.com/todos?_limit=5")
      .then(res => res.json())
      .then(data => this.todos = data)
      .catch(e => console.log(e))
    }
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body{
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }

  .btn{
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }

  .btn:hover{
    background: #666;
  }
</style>
