<template>
  <div>
    <form class="input__block" @submit.prevent><h4>Создать заметку</h4>
    <input class="input__form" v-model="title" placeholder="Название заметки">
    <input class="input__form"  v-model="body" placeholder="Описание заметки">
      <button class="input__btn" @click="createTask">Сохранить</button>
    </form>

    <div class="new-tasks">
      <h4>Новые заметки</h4>
      <div class="task" v-for="task in tasks.filter(task => !task.status)" :key="task.id">
        <div>
          <div class="task__indicator" @click="toggleStatus(task)"> <div class="task__idicator-status" v-if="task.status"></div></div>
          <div > <strong>Название</strong> {{ task.title }}</div>
          <div><strong>Описание</strong> {{ task.body }}</div>
        </div>
        <div class="task__btns">
          <button class="task__btn" @click="deleteTask(task.id)">Удалить</button>
          <button class="task__btn" @click="editTask(task)">Изменить</button>
        </div>
      </div>  
    </div>

    <div class="done-tasks">
      <h4>Завершенный заметки</h4>
      <div class="task" v-for="task in tasks.filter(task => task.status)" :key="task.id">
        <div> 
          <div class="task__indicator" @click="toggleStatus(task)"> <div class="task__idicator-status" v-if="task.status"></div></div>
          <div ><strong>Название</strong> {{ task.title }} </div>
          <div><strong>Описание</strong> {{ task.body }}</div>
        </div>
        
        <div class="task__btns">
          <button class="task__btn" @click="deleteTask(task.id)">Удалить</button>
          <button class="task__btn" @click="editTask(task)">Изменить</button>
        </div>
      </div>  
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      title: '', 
      body: '',  
      isEditing: false, 
      editingTaskId: null, 
      tasks: []
    };
  },
  methods: {
    createTask() {
      if (this.isEditing) {
        
        const taskIndex = this.tasks.findIndex(task => task.id === this.editingTaskId);
        if (taskIndex !== -1) {
          this.tasks[taskIndex].title = this.title;
          this.tasks[taskIndex].body = this.body;
        }
        this.isEditing = false;
        this.editingTaskId = null;
      } else {
        
        const newTask = {
          id: Date.now(),
          title: this.title,
          body: this.body,
          status: false
        };
        this.tasks.push(newTask);
      }
      this.saveTasksToLocalStorage(); 
      this.title = '';
      this.body = '';
    },
    toggleStatus(task) {
      task.status = !task.status;
      this.saveTasksToLocalStorage();
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
      this.saveTasksToLocalStorage(); 
    },
    editTask(task) {
      this.title = task.title;
      this.body = task.body;
      this.isEditing = true;
      this.editingTaskId = task.id;
    },
    saveTasksToLocalStorage() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    loadTasksFromLocalStorage() {
      const tasks = localStorage.getItem('tasks');
      if (tasks) {
        this.tasks = JSON.parse(tasks);
      }
    }
  },
  mounted() {
    this.loadTasksFromLocalStorage();
  }
};
</script>

<style scoped>

.input__block {
  color:#1A2902;
  border-radius: 15px;
  margin-top: 5px;
  padding: 15px;
  border: 3px solid #344C11;
  background-color: #778D45;
  min-height: 100px;

}

.input__form{
  padding: 5px;
  border-radius: 9px;
  margin-left: 5px;
  border: 2px solid #344C11;
  background-color: #d7e9c3;
  color:#1A2902;


}

.input__btn {
  padding: 5px;
  background-color: #AEC09A;
  border-radius: 9px;
  margin-left: 5px;
  border: 2px solid #344C11;
}

.task__indicator {
  position: absolute;
  padding: 3px;
  margin-left: -28px;
  margin-top: 7px;
  width: 20px;
  height: 20px;
  border-radius: 50px;
  background-color: #d7e9c3;
  box-sizing: border-box;
}

.task__idicator-status {
  height: 100%;
  width: 100%;
  border-radius: 50px;
  background-color: #344C11;
  
}

.task {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  padding-left: 40px;
  margin-top: 5px;
  border: 2px solid #1A2902;
  border-radius: 15px;
  background-color: #AEC09A;
  color:#1A2902;

}

.new-tasks {
  color:#1A2902;
  border-radius: 15px;
  margin-top: 5px;
  min-height: 200px;
  padding: 15px;
  border: 3px solid #583E26;
  background-color: #d49714;
}
.done-tasks {
  color:#1A2902;
  border-radius: 15px;
  margin-top: 5px;
  padding: 15px;
  border: 3px solid #344C11;
  background-color: #778D45;
  min-height: 200px;

}

.task__btns {
  display: flex;
  flex-direction: column;
}
.task__btn {
  padding: 2px 5px;
  background-color: #AEC09A;
  border-radius: 9px;
  margin-top: 5px;
  border: 2px solid #344C11;
}


</style>