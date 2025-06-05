<template>
  <div class="todo-app">
    <h1>To-Do List</h1>
    
   
    <div class="add-task">
      <input 
        v-model="newTask" 
        @keyup.enter="addTask"
        placeholder="Введите новую задачу."
      >
      <button @click="addTask">+</button>
    </div>
    
    
    <ul class="task-list">
      <li 
        v-for="(task, index) in tasks" 
        :key="index"
        :class="{ done: task.done }"
      >
        <span @click="toggleTask(index)">{{ task.text }}</span>
        <button @click="removeTask(index)">×</button>
      </li>
    </ul>
    
    
    <p v-if="tasks.length === 0" class="empty">Нет задач</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: []
    }
  },
  mounted() {
    
    const saved = localStorage.getItem('tasks')
    if (saved) this.tasks = JSON.parse(saved)
  },
  methods: {
    addTask() {
      if (this.newTask.trim() === '') return
      
      this.tasks.push({
        text: this.newTask,
        done: false
      })
      
      this.newTask = ''
      this.saveTasks()
    },
    removeTask(index) {
      this.tasks.splice(index, 1)
      this.saveTasks()
    },
    toggleTask(index) {
      this.tasks[index].done = !this.tasks[index].done
      this.saveTasks()
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    }
  }
}
</script>

<style>
    .todo-app {
    max-width: 500px;
    margin: 20px auto;
    padding: 20px;
    font-family: Arial, sans-serif;
    background-color: #333;
    border-radius: 10px;
    }

    h1 {
    color: #ffffff;
    text-align: center;
    }

    .add-task {
    display: flex;
    margin-bottom: 20px;
    }

    .add-task input {
    flex: 1;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px 0 0 4px;
    }

    .add-task button {
    padding: 10px 15px;
    background: #4CAF50;
    color: white;
    border: none;
    border-radius: 0 4px 4px 0;
    cursor: pointer;
    }

    .task-list {
    list-style: none;
    padding: 0;
    }

    .task-list li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    margin-bottom: 5px;
    background: #f9f9f9;
    border-radius: 4px;
    }

    .task-list li.done {
    opacity: 0.6;
    }

    .task-list li.done span {
    text-decoration: line-through;
    }

    .task-list li span {
    cursor: pointer;
    }

    .task-list li button {
    background: #ff4444;
    color: white;
    border: none;
    border-radius: 50%;
    width: 25px;
    height: 25px;
    cursor: pointer;
    }

    .empty {
    text-align: center;
    color: #888;
    }
</style>