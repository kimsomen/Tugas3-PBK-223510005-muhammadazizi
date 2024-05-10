<template>
  <div class="container">
    <h1 style="text-align: center;">To Do List</h1>
    <div class="add-task">
      <input type="text" v-model="newTask" placeholder="Add new task...">
      <input type="text" v-model="newCategory" placeholder="Category...">
      <button @click="addTask">Add</button>
    </div>

    <table class="task-table">
      <tbody>
        <tr v-for="(task, index) in filteredTasks" :key="index" :class="{ 'completed': task.completed }">
          <td><input type="checkbox" v-model="task.completed" class="checkbox"></td>
          <td>
            <div v-if="!task.editing" class="task-text">{{ task.title }}</div>
            <div v-else class="edit-mode">
              <input v-model="task.title" class="edit-input">
            </div>
          </td>
          <td>
            <div v-if="!task.editing" class="task-text">{{ task.category }}</div>
            <div v-else class="edit-mode">
              <input v-model="task.category" class="edit-input">
            </div>
          </td>
          <td>
            <div v-if="!task.editing" class="button-group">
              <button class="edit-btn" @click="editTask(index)">Edit</button>
              <button class="delete-btn" @click="deleteTask(index)">Delete</button>
            </div>
            <div v-else class="button-group">
              <button class="update-btn" @click="updateTask(index)">Update</button>
              <button class="cancel-btn" @click="cancelTask(index)">Cancel</button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>      

    <br>
    
    <div class="filters">
      <select v-model="selectedCategory" @change="filterByCategory">
        <option value="">All Categories</option>
        <option v-for="category in categories" :key="category">{{ category }}</option>
      </select>
      <input type="text" v-model="searchQuery" placeholder="Search tasks...">
    </div>
          
    <button class="filter-btn" @click="toggleShowIncomplete">
      {{ showIncomplete ? 'Show All Tasks' : 'Show Incomplete Tasks Only' }}
    </button>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      newTask: '',
      newCategory: '',
      showIncomplete: true,
      selectedCategory: '',
      searchQuery: ''
    };
  },
  computed: {
    filteredTasks() {
      let filtered = this.tasks.filter(task => {
        if (this.showIncomplete && task.completed) return false;
        if (this.selectedCategory && task.category !== this.selectedCategory) return false;
        if (this.searchQuery && !task.title.toLowerCase().includes(this.searchQuery.toLowerCase())) return false;
        return true;
      });
      return filtered;
    },
    categories() {
      let categories = new Set(this.tasks.map(task => task.category));
      return Array.from(categories);
    }
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push({ title: this.newTask, completed: false, category: this.newCategory || 'Uncategorized' });
        this.newTask = '';
        this.newCategory = '';
      }
    },
    cancelTask(index) {
      this.tasks[index].editing = false;
    },
    editTask(index) {
      this.tasks[index].editing = true;
    },
    updateTask(index) {
      this.tasks[index].editing = false;
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    toggleShowIncomplete() {
      this.showIncomplete = !this.showIncomplete;
    },
    filterByCategory() {
    }
  },
  created() {
  },
  mounted() {
  },
  watch: {
    tasks: {
      handler: function(val, oldVal) {
      },
      deep: true
    }
  },
  
  refs: {
    myRef: HTMLElement
  }
};
</script>

<style scoped>
@media (max-width: 768px) {
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #0c0c0f;
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: 'Courier New', Courier, monospace;
  }

  .container {
    width: 300px;
  }

  input[type="text"] {
    width: calc(100% - 75px);
  }
}
</style>
