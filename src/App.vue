<template>
  <h4 class="bg-primary text-white text-center p-2">
      {{name }}'s TODO'
 </h4>
 <div class="container-fluid p-4">
  <div class="row">
    <div class="col fw-bold">Task</div>
    <div class="col-2 fw-bold">Done</div>
  </div>
  <div class="row" v-if="filteredTasks.length == 0">
    <div class="col text-center">
      <b>Nothing to do. Hurrah!</b>
    </div>
  </div>
  <div v-else>
    <div class="row" v-for="t in filteredTasks" v-bind:key="t.action">
    <div class="col">{{t.action}}</div>
    <div class="col-2">
      <input type="checkbox" v-model="t.done"  class="form-check-input">
    </div></div>
 </div>
 <div class="row bg-secondary py-2 mt-2 text-white">
  <div class="col text-center">
    <input type="checkbox" v-model="hideCompleted" class="form-check-input" />
    <label class="form-check-label font-weight-bold">
       Hide completed tasks
    </label>
  </div>
 </div>
  </div>
 <div class="row py-2">
  <div class="col">
    <input v-model="newItemText" class="form-control" />
  </div>
  <div class="col-2">
    <button class="btn btn-primary" v-on:click="addNewTodo">Add</button>
  </div>
 </div>
  <div class="row">
    <div class="col text-center">
      <button class="btn btn-sm btn-warning"
      v-on:click="deleteCompleted">
      Delete Completed
      </button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      name: 'Wabs',
      tasks: [],
      hideCompleted: true,
      newItemText: ""
    }
  },
  computed: {
    filteredTasks() {
      return this.hideCompleted ? this.tasks.filter(x => !x.done) : this.tasks;
    }
  },
  methods: {
    addNewTodo() {
      this.tasks.push({
        action: this.newItemText,
        done: false
      });
      this.storeData();
      this.newItemText = "";
    },
    storeData() {
    localStorage.setItem("todos", JSON.stringify(this.tasks));
    },
    deleteCompleted() {
      this.tasks = this.tasks.filter(t => !t.done);
      this.storeData();
    }
  },
  created() {
    let data = localStorage.getItem('todos');
    if(data) {
      this.tasks = JSON.parse(data);
    }
  }
}
</script>

