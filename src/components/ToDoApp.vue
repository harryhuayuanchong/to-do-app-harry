<template>
  <div class="container">
    <h2>Harry To Do App</h2>

    <!-- Generate a bootstrap table -->

    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Enter task" class="form-control">
      <button @click="submitTask" class="btn btn-dark rounded-3">Submit</button>
    </div>

    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">Modification</th>
          <th scope="col" class="text-center">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{'finished': task.status === 'finished'}">{{task.name}}</span>
          </td>
          <td>
            <span @click="changeStatus(index)" class="pointer"
              :class="{'text-danger': task.status === 'to-do',
              'text-warning': task.status === 'in-progress',
              'text-success': task.status === 'finished'
              }"
            >
              {{firstCharUpper(task.status)}}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
      task: '',
      editedTask: null,
      availableStatuses: ['to-do', 'in-progress', 'finished'],

      tasks:[
      {
        name: 'Vue project',
        status: 'to-do'
      },
      {
        name: 'Hit to the gym',
        status: 'in-progress'
      }
    ]
    }
  },

  methods: {
    submitTask(){
      if(this.tasks.length === 0) return;

      if(this.editedTask === null){
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        });
      }else{
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = '';
    },

    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    deleteTask(index){
      this.tasks.splice(index, 1);
    },

    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },

    firstCharUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer{
  cursor: pointer;
}
.finished{
  text-decoration: line-through;
}
</style>
