<template>
  <div class="container">
    <h2 class="text-center mt-5">
      Todo Vue App
    </h2>

    <!-- Input -->
    <div class="d-flex">
      <input v-model="taskInput" type="text" placeholder="Enter Task" class="form-control" />
      <button @click="submitTask" v-on:keyup.enter="submitTask" class="button btn-warning rounded-3">Submit</button> 
    </div>

    <!-- Task Table -->
    {{log(tasks.length)}}
    <table v-if="tasks.length !== 0" class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td :class="{'finished' : task.status === 'finished'}" style="width:50vw">
            {{firstCharUpper(task.name)}}
          </td>
          <td style="width: 120px">
            <span @click="changeStatus(index)" class="pointer" 
              :class="{'text-danger' : task.status === 'to-do',
                'text-warning' : task.status === 'in-progress',
                'text-success' : task.status === 'finished'
              }"
            >
              {{firstCharUpper(task.status)}}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen pointer"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-else class="text-center py-5">
      <span>No data available here</span>
    </div>
  </div>

</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  data() {
    return {
      taskInput: "",
      editedTaskTask: null,
      availableStatuses: ['to-do', 'in-progress', 'finished'],

      tasks: [
        {
          name: 'Buy the banana',
          status: 'to-do'
        },
        {
          name: 'Eat 1kg cocoa',
          status: 'in-progress'
        },
        {
          name: 'Drink water 8 Liter',
          status: 'finished'
        },
      ],

      log: console.log
    }  
  },

  methods: {
    onEnter: function() {
       this.props.msg = 'on enter event';
    },

    submitTask() {
      if (this.taskInput.length === 0) return;

      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.taskInput
        this.editedTask = null
      } else {
        this.tasks.push({
          name: this.taskInput,
          status: "to-do" 
        })
      }

      this.taskInput = ""
    },

    deleteTask(index) {
      // splice()
      // 1st parameter is index position to remove
      // 2nd parameter is integer whice specifies how manys items we want to delete starting from that index
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.taskInput = this.tasks[index].name;
      this.editedTask = index
    },

    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status)
      if(++newIndex > 2) newIndex = 0
      this.tasks[index].status = this.availableStatuses[newIndex]
    },

    firstCharUpper(str) {
        return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .pointer {
    cursor: pointer;
  }

  .finished {
    text-decoration: line-through;
  }
</style>
