<template>
  <div class="modal fade" id="edit-title" tabindex="-1" role="dialog" aria-labelledby="edit-title" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" style="color:#616cf5" id="exampleModalLongTitle">Edit your title</h5>
          <button  type="button" class="close" data-dismiss="modal" aria-label="Close">
            <img src="../assets/icons/xBlack.svg">
          </button>
        </div>
        <div class="modal-body">
          <input v-model="currentTitle" class="input" >
        </div>
        <div class="modal-footer">
          <button @click.prevent="save()"   type="button" class="add-task">Save</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import $ from 'jquery'
export default {
  name: "EditTitle",
  props:['title'],
  data(){
    return {
      currentTitle:'Manage your task'
    }
  },
  methods:{
    save(){
      this.$emit('editedTitle', this.currentTitle)
      $('#edit-title').modal('hide')

    }
  },

  watch:{
    title:function(value){
      this.currentTitle = value
    },
    deep:true
  }

}
</script>

<style scoped>
.input{
  width: 100%;
}
</style>