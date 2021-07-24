<template>
  <div class="modal fade" id="edit" tabindex="-1" role="dialog" aria-labelledby="edit" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" style="color:#616cf5" id="exampleModalLongTitle">Edit you task</h5>
          <button  type="button" class="close" data-dismiss="modal" aria-label="Close">
            <img src="../assets/icons/xBlack.svg">
          </button>
        </div>
        <div class="modal-body">
          <label>Task name</label>
          <input v-model="currentTask.name" class="input" >
          <label>Sub task name</label>
          <input v-model="subTask.name" class="">
        </div>
        <div class="modal-footer">
          <button @click="onSubmit" type="button" class="add-task">Save</button>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: "EditTask",
  props:['edit_task'],
  data(){
    return{
      currentTask:'',
      subTask:{
        id:Date.now(),
        name:''
      },
    }
  },
  methods:{
    onSubmit(){
      if(this.subTask.name !== ''){

        this.currentTask.subtask.push(this.subTask)
      }
      this.$emit('editedTask',this.currentTask)
      this.subTask={
        id:Date.now(),
        name:''
      }
    }
  },

  watch:{
    edit_task(newTask){
      this.currentTask = Object.assign({}, newTask)
    }
  }
}
</script>

<style scoped>
.input{
  width: 100% !important;
}

</style>