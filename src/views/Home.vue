<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Mukul is making first Vue.js"/> -->
    <!-- <Header/> -->
    <AddToDo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Todos from '../components/Todos.vue'
// import Header from '../components/layout/Header'
import AddToDo from '../components/AddToDo'
import axios from 'axios'

export default {
  name: 'Home',
  components: {
    Todos,
    // Header,
    AddToDo
    // HelloWorld
  },
  data() {
    return {
      todos:[]
    }
  },
  methods:{
    deleteTodo(id){
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(() => this.todos = this.todos.filter((todo)=>todo.id!=id))
        .catch(err=>console.log(err))

// this.todos = this.todos.filter((todo)=>todo.id!=id)
    },
    addTodo(newTodo){
      const { title, completed} = newTodo;
      axios.post('https://jsonplaceholder.typicode.com/todos',{
        title,
        completed
      })
        .then(res => this.todos = [ ...this.todos, res.data])
        .catch(err=>console.log(err))
      // this.todos = [ ...this.todos, newTodo];
    }
  },
  created(){
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=10')
      .then(res=>this.todos = res.data)
      .catch(err=>console.log(err))
  }
}
</script>

<style>

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  line-height: 1.4;
}

.btn{
  display: inline-block;
  border: none;
  background: #555;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover{
  background: #666;
}
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
