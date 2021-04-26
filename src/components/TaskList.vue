<template>
  <div>
    <input
      type="text"
      class="task-input"
      placeholder="Add task here"
      v-model="newTask"
      @keyup.enter="addTask"
    />
    <div
      v-for="(task, index) in tasksFiltered"
      :key="task.id"
      class="task-item"
    >
      <div class="task-item-left">
        <input type="checkbox" v-model="task.completed" />
        <div
          v-if="!task.editing"
          @dblclick="editTask(task)"
          class="task-item-label"
          :class="{ completed: task.completed }"
        >
          {{ task.title }}
        </div>
        <input
          v-else
          class="task-item-edit"
          type="text"
          v-model="task.title"
          @blur="doneEdit(task)"
          @keyup.enter="doneEdit(task)"
          @keyup.esc="cancelEdit(task)"
          v-focus
        />
      </div>
      <div class="remove-task" @click="removeTask(index)">
        &times;
      </div>
    </div>
    <div class="extra-container">
      <div>
        <label
          ><input
            type="checkbox"
            :checked="!anyRemaining"
            @change="checkAllTasks"
          /> Mark all as completed</label
        >
      </div>
      <div>{{ remaining }} items left</div>
    </div>

    <div class="extra-container">
      <div>
        Filters:  <button :class="{ active: filter == 'all' }" @click="filter = 'all'">
          All
        </button>
        <button
          :class="{ active: filter == 'active' }"
          @click="filter = 'active'"
        >
          Active
        </button>
        <button
          :class="{ active: filter == 'completed' }"
          @click="filter = 'completed'"
        >
          Completed
        </button>
      </div>
      <div v-if="showClearCompletedButton" @click="clearCompleted">
        <button>Delete completed tasks</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "task-list",
  data() {
    return {
      newTask: "",
      idForTask: 4,
      beforeEditCache: "",
      filter: "all",
      tasks: [
        {
          id: 1,
          title: "Wash the car",
          completed: false,
          editing: false
        },
        {
          id: 2,
          title: "Hoover the bedroom",
          completed: false,
          editing: false
        },
        {
          id: 3,
          title: "Cook the dinner",
          completed: false,
          editing: false
        }
      ]
    };
  },
  computed: {
    remaining() {
      return this.tasks.filter(task => !task.completed).length;
    },
    anyRemaining() {
      return this.remaining != 0;
    },
    tasksFiltered() {
      if (this.filter == "all") {
        return this.tasks;
      } else if (this.filter == "active") {
        return this.tasks.filter(todo => !todo.completed);
      } else if (this.filter == "completed") {
        return this.tasks.filter(todo => todo.completed);
      }

      return this.tasks;
    },
    showClearCompletedButton() {
      return this.tasks.filter(task => task.completed).length > 0;
    }
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  methods: {
    addTask() {
      if (this.newTask.trim().length == 0) {
        return;
      }

      this.tasks.push({
        id: this.idForTask,
        title: this.newTask,
        completed: false,
        editing: false
      });

      (this.newTask = ""), this.idForTask++;
    },
    editTask(task) {
      this.beforeEditCache = task.title;
      task.editing = true;
    },
    doneEdit(task) {
      if (task.title.trim().length == "") {
        task.title = this.beforeEditCache;
      }
      task.editing = false;
    },
    cancelEdit(task) {
      task.title = this.beforeEditCache;
      task.editing = false;
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
    checkAllTasks() {
      this.tasks.forEach(task => (task.completed = event.target.checked));
    },
    clearCompleted() {
      this.tasks = this.tasks.filter(task => !task.completed);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.task-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
}

.task-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.remove-task {
  cursor: pointer;
  margin-left: 14px;
}

.task-item-left {
  display: flex;
  align-items: center;
}

.task-item-label {
  padding: 10px;
  border: 1px solid white;
}

.task-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
}

.completed {
  text-decoration: line-through;
  color: darkgrey;
}

.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgrey;
  padding-top: 14px;
  margin-bottom: 14px;
}

button {
  font-size: 14px;
  background-color: white;
  appearance: none;
}

button:hover {
  background: #cdf9da;
}

.active {
  background: #cdf9da;
}
</style>
