<template>
  <div class="container" style="max-width: 600px">
    <h2 class="mt-5">Todo List</h2>

    <div class="mt-5 flexxx">
      <input
        v-model="task"
        placeholder="Enter task"
        class="input"
        type="text"
      />
      <button class="btn warning" @click="submitTask">SUBMIT</button>
    </div>

    <div class="mt-5">
      <!-- <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col" style="width: 120px">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead> -->
      <div>
        <div class="flexxx" v-for="(task, index) in tasks" :key="index">
          <div>
            <h3 :class="{ 'line-through': task.status === 'finished' }">
              {{ task.name }}
            </h3>
          </div>

          <div class="flexxx">
            <div>
              <h3
                class="pointer noselect"
                @click="changeStatus(index)"
                :class="{
                  'text-danger': task.status === 'To-do',
                  'text-success': task.status === 'Finished',
                  'text-warning': task.status === 'In-progress',
                }"
              >
                <button class="btn primary">{{ task.status }}</button>
              </h3>
            </div>
            <div class="text-center">
              <div @click="deleteTask(index)">
                <button class="btn error">Delete</button>
              </div>
            </div>
            <div class="text-center">
              <div @click="editTask(index)">
                <button class="btn success">Edit</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "Go to market",
          status: "To-do",
        },
        {
          name: "Complete Assignment",
          status: "In-progress",
        },
      ],
    };
  },

  methods: {
    /**
     * Change status of task by index
     */
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },

    /**
     * Deletes task by index
     */
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    /**
     * Edit task
     */
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    /**
     * Add / Update task
     */
    submitTask() {
      if (this.task.length === 0) return;

      /* We need to update the task */
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        /* We need to add new task */
        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }

      this.task = "";
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}
.line-through {
  text-decoration: line-through;
}

.flexxx {
  display: flex;
  justify-content: space-between;
}
</style>