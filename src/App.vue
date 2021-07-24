<template>
 <div class="container p-5">
  <div class="row">
    <div class="col-lg-6 main-box m-auto">
      <div class="d-flex align-items-center justify-content-between mb-3">

        <h1 class="title">{{title}}</h1>
        <span style="color:#616cf5; cursor:pointer;" data-toggle="modal" data-target="#edit-title">edit</span>
      </div>
      <input class="input w-100" placeholder="Search" v-model="search">
      <div>
        <ul v-if="filteredTasks.length" class="p-0">
          <li v-for="todo in filteredTasks" :key="todo.id" class="list-item">
          <div class="d-flex justify-content-between align-items-center">
            <span :class="{done:todo.done === true}" class="todo-name"> {{todo.name}}</span>
            <div>
              <span class="text-danger" @click="deleteFromTodo(todo.id)"><i class="fas fa-trash-alt"></i></span>
              <span class="text-success" @click="done(todo)"><i class="far fa-check-circle"></i></span>
              <span class="three-dot" @click="edit_task=todo" data-toggle="modal" data-target="#edit"><i class="fas fa-ellipsis-h"></i></span>
            </div>
          </div>
            <div class="sub-name d-flex justify-content-between align-items-center" v-for="sub in todo.subtask" :key="sub.id">
              <span>{{sub.name}}</span>
              <img @click="deleteSubTask(todo, sub)" src="./assets/icons/xBlack.svg" style="width:14px; height:14px; cursor:pointer;">
            </div>
          </li>

        </ul>
        <div class="mb-4" v-else>
          <span>You don't have any tasks</span>
        </div>
      </div>

      <form>
        <input class="input" v-model="newTodo.name" placeholder="Enter you task">
        <button class="add-task" @click.prevent="addTask">Add task</button>
      </form>
    </div>
  </div>


  <EditTask
    v-bind:edit_task="edit_task"
    v-on:editedTask="editedTask"
    />
   <EditTitle
    v-bind:title="title"
    v-on:editedTitle="editedTitle"
   />





  </div>
</template>

<script>
import $ from 'jquery';
import EditTask from "./components/EditTask";
import EditTitle from "./components/EditTitle";
export default {
  name: 'App',
  components:{
    EditTask,
    EditTitle,
  },

  data(){
    return{
      title:'Manage your tasks',
      newTodo:{
        id: Date.now(),
        name:'',
        done:false,
        subtask:[]
      },

      todolist:[],
      edit_task:'',
      search:''
    }
  },
  computed:{
    filteredTasks(){
      return this.todolist.filter((item)=>item.name.toLowerCase().includes(this.search.toLowerCase()))
    }
  },
  methods:{
    deleteSubTask(todo, sub){
      todo.subtask = todo.subtask.filter((item)=>item.id!== sub.id)
    },
    addTask(){
      if(this.newTodo.name === ''){
        alert("Please enter your task")
      }
      else{
        this.todolist.push(this.newTodo)
      }
      this.newTodo={
            id:Date.now(),
            name:'',
            done:false,
            subtask:[]
      }
    },
    deleteFromTodo(id){
      this.todolist = this.todolist.filter((item)=>item.id !== id)
    },
    done(item){
      item.done = !item.done
      return item
    },
    editedTask(edited){
      const idx = this.todolist.findIndex((item)=>item.id === edited.id)
      this.todolist[idx] = edited;
      $('#edit').modal('hide')
    },
    editedTitle(edited){
      this.title = edited
    },


  },
  mounted(){
    if (sessionStorage.getItem('todolist')) this.todolist = JSON.parse(sessionStorage.getItem('todolist'));
    if(sessionStorage.getItem('title')) this.title = sessionStorage.getItem('title')
  },

  watch:{
    todolist:{
     handler(){
       sessionStorage.setItem('todolist', JSON.stringify(this.todolist));
     },
      deep:true
    },
    search:{
      handler(){
        sessionStorage.setItem('todolist',JSON.stringify(this.filteredTasks))
      },
      deep:true
    },
    title:{
      handler(){
        sessionStorage.setItem('title', this.title)
      }
    }
  }

}
</script>

<style>
.three-dot{
  color:grey;
}
.main-box{
  background: #FFFFFF;
  padding:20px  !important;
  box-shadow: 2px 11px 35px rgba(0, 0, 0, 0.1);
}
.input{
  width: 70% !important;
}
.list-item{
  padding:5px 0;
  font-weight: 600;
  list-style: none;
}
.sub-name{
  font-weight: 400;
  padding:5px 15px;

  font-size: 14px;
  color:#222;
}

.list-item span{
  cursor:pointer;
  margin-right: 15px;
}
.title{
  font-size: 22px;
}
.done{
  text-decoration: line-through;
  color:green;
}
</style>
