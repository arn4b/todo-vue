<template>
  <div class="container" style="max-width: 600px">
    <h2 class="mt-5">Todo List</h2>

    <div class="mt-5 flexxx">
      <input
        v-model="task"
        placeholder="Enter task"
        class="input mr-2"
        type="text"
      />
      <button class="btn warning" @click="submitTask">SUBMIT</button>
    </div>

    <div class="mt-5">
      <div>
        <div class="flexxx my-5" v-for="(task, index) in tasks" :key="index">
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
  name: "Todo App",
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
          status: "to-do",
        },
        {
          name: "Complete Assignment",
          status: "in-progress",
        },
      ],
    };
  },

  methods: {
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
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

.line-through {
  text-decoration: line-through;
}

.flexxx {
  display: flex;
  justify-content: space-between;
}
</style>