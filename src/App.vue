<template>
  <div class="container py-5 my-5">
    <h4 v-if="completedTasks.length > 0" class="list-group list-group-numbered">
      Completed Tasks
    </h4>
    <ol v-if="completedTasks.length > 0" class="list-group list-group-numbered">
      <li
        v-for="(task, i) in completedTasks"
        :key="task.name + '-' + i"
        class="list-group-item d-flex justify-content-between align-items-start"
        :class="{ 'list-group-item-success': task.completed }"
      >
        <div class="ms-2 me-auto">
          <div class="fw-bold">
            {{ task.completed ? "Completed" : "Not Completed" }}
          </div>
          {{ task.name }}
        </div>
        <input type="checkbox" v-model="task.completed" />
      </li>
    </ol>

    <template v-if="pendingTasks.length > 0">
      <h4 class="mt-5">Pending Tasks</h4>
      <ol class="list-group list-group-numbered">
        <li
          v-for="(task, i) in pendingTasks"
          :key="task.name + '-' + i"
          class="list-group-item d-flex justify-content-between align-items-start list-group-item-warning"
        >
          <div class="ms-2 me-auto">
            <div class="fw-bold">
              {{ task.completed ? "Completed" : "Not Completed" }}
            </div>
            {{ task.name }}
          </div>
          <input type="checkbox" v-model="task.completed" />
        </li>
      </ol>
    </template>

    <input
      type="text"
      v-model="form.name"
      class="form-control mt-5"
      @keydown.enter.stop="addTodoToTasks"
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        name: "",
        completed: false,
      },
      tasks: [
        {
          name: "Go shopping",
          completed: true,
        },
        {
          name: "Do programming",
          completed: false,
        },
        {
          name: "Take the baby to school",
          completed: false,
        },
      ],
    };
  },
  created() {
    if (localStorage.getItem("tasks")) {
      this.tasks = JSON.parse(window.atob(localStorage.getItem("tasks")));
    }
  },
  watch: {
    tasks: {
      handler(newValue, oldValue) {
        localStorage.setItem("tasks", window.btoa(JSON.stringify(this.tasks)));
      },
      deep: true,
    },
  },
  computed: {
    completedTasks() {
      return this.tasks.filter((task) => task.completed === true);
    },
    pendingTasks() {
      return this.tasks.filter((task) => task.completed === false);
    },
  },
  methods: {
    addTodoToTasks() {
      if (this.form.name) {
        this.tasks.push(JSON.parse(JSON.stringify(this.form)));
        this.form.name = "";
      } else {
        alert("Please provide a task");
      }
    },
  },
};
</script>

<style>
/* Your styles here */
</style>
