<template>
  <div class="card">
    <div class="card-header text-center" :class="titleClasses">
      <h4>{{ title }}</h4>
    </div>
    <div class="card-body">
      <Task
          v-for="task in tasks"
          :key="task.id"
          :task="task"
          :alertColor="alertColor"/>
    </div>
    <div class="card-footer" v-if="newTasks">
      <NewTask @new-task="newTask"/>
    </div>
  </div>
</template>

<script>
import Task from "./Task";
import NewTask from "./NewTask";

export default {
  name: "StatusCard",
  components: {
    Task,
    NewTask
  },
  props: {
    title: String,
    titleClasses: String,
    status: Number,
    newTasks: Boolean,
    tasks: Array
  },
  emits: {
    "new-task": (task) => {
      if ("status" in task === false) {
        console.warn("StatusCardComponent: Jede Aufgabe muss ein Status-Attribut haben")
        return false
      }
      return true

    }
  },
  computed: {
    alertColor() {
      switch (this.status) {
        case 0:
          return "secondary"
        case 1:
          return "primary"
        case 2:
          return "success"
      }
      return "danger"
    }
  },
  methods: {
    newTask(task) {
      task.status = this.status
      this.$emit("new-task", task)
    }
  }
}
</script>

<style scoped>

</style>