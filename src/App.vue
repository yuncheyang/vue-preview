<template>
  <div class="container">
    <input v-model="newTask" class="input" placeholder="输入任务..." @keyup.enter="addTask" />
    <button class="submit-btn" @click="addTask">Submit</button>

    <div class="task-list">
      <div class="task-column">
        <h3>进行中的任务</h3>
        <div
          v-for="(task, index) in ongoingTasks"
          :key="index"
          class="task"
          @mouseenter="task.hover = true"
          @mouseleave="task.hover = false"
        >
          <span>{{ task.name }}</span>
          <div v-if="task.hover" class="actions">
            <button class="btn complete" @click="markAsDone(index)">完成</button>
            <button class="btn edit" @click="editTask(index)">修改</button>
            <button class="btn delete" @click="deleteTask(index, 'ongoing')">删除</button>
          </div>
        </div>
      </div>

      <div class="task-column">
        <h3>已完成的任务</h3>
        <div
          v-for="(task, index) in completedTasks"
          :key="index"
          class="task completed"
          @mouseenter="task.hover = true"
          @mouseleave="task.hover = false"
        >
          <span>{{ task.name }}</span>
          <div v-if="task.hover" class="actions">
            <button class="btn delete" @click="deleteTask(index, 'completed')">删除</button>
          </div>
        </div>
      </div>
    </div>

    <div v-if="editingTask !== null" class="modal">
      <input v-model="editText" class="input" />
      <button class="btn confirm" @click="confirmEdit">确认</button>
      <button class="btn cancel" @click="cancelEdit">取消</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const newTask = ref('')
const ongoingTasks = ref([])
const completedTasks = ref([])
const editingTask = ref(null)
const editText = ref('')

const addTask = () => {
  if (newTask.value.trim() !== '') {
    ongoingTasks.value.push({ name: newTask.value, hover: false })
    newTask.value = ''
  }
}

const markAsDone = (index) => {
  completedTasks.value.push(ongoingTasks.value[index])
  ongoingTasks.value.splice(index, 1)
}

const deleteTask = (index, list) => {
  if (list === 'ongoing') {
    ongoingTasks.value.splice(index, 1)
  } else {
    completedTasks.value.splice(index, 1)
  }
}

const editTask = (index) => {
  editingTask.value = index
  editText.value = ongoingTasks.value[index].name
}

const confirmEdit = () => {
  if (editingTask.value !== null) {
    ongoingTasks.value[editingTask.value].name = editText.value
    editingTask.value = null
  }
}

const cancelEdit = () => {
  editingTask.value = null
}
</script>

<style>
/* 让整个页面居中 */
.container {
  width: 1000px;
  margin: auto;
  margin-left: 100px;
  margin-top: -250px;
  text-align: center;
  background: #f9f9f9;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
}
.input {
  padding: 10px;
  width: 80%;
  margin: 10px 0;
  border: 1px solid #ddd;
  border-radius: 5px;
}
.submit-btn {
  padding: 10px 15px;
  background: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
.task-list {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}
.task-column {
  width: 45%;
}
.task {
  padding: 10px;
  background: #fff;
  margin: 5px 0;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
}
.completed {
  background: #d4edda;
}
.actions {
  display: flex;
  gap: 5px;
}
.btn {
  padding: 5px 10px;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}
.complete {
  background: #28a745;
  color: white;
}
.edit {
  background: #ffc107;
  color: white;
}
.delete {
  background: #dc3545;
  color: white;
}
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
  padding: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
  border-radius: 10px;
}
.confirm {
  background: #007bff;
  color: white;
}
.cancel {
  background: #6c757d;
  color: white;
}
</style>
