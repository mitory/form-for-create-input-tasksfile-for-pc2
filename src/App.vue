<template>
  <div class="container">
    <div class="wrapper">
      <input type="text" placeholder="Название задачи" v-model="tasks[index].title">
      <textarea name="text" id="text" placeholder="Описание задачи" v-model="tasks[index].text"></textarea>
      <textarea name="input" id="input" placeholder="Формат входных данных" v-model="tasks[index].input"></textarea>
      <textarea name="output" id="output" placeholder="Формат выходных данных" v-model="tasks[index].output"></textarea>
      <div class="input-time">
        <span>Время (в секундах): </span><input type="text" v-model="tasks[index].time_limit">
      </div>
      <p class="counter-tasks">Задача номер: {{ index + 1 }} / {{ tasks.length }}</p>
      <div class="button-wrapper">
        <button 
          class="button" 
          title="Предыдущая задача"
          :disabled="index === 0"
          @click="index--">{{"<<"}}</button>
        <button 
          class="button" 
          :disabled="index !== tasks.length - 1"
          title="Добавить задачу"
          @click="addTasks(); index = tasks.length - 1;">+</button>
        <button 
          class="button" 
          title="Удалить текущую задачу"
          @click="removeTask(); index = 0;">-</button>
        <button 
          class="button" 
          title="Следующая задача"
          :disabled="index === tasks.length - 1"
          @click="index++">{{">>"}}</button>
      </div>
    </div>
    <button class="button" @click="fileUpload">Скачать файл</button>
  </div>
</template>

<script>
import { ref } from 'vue';

  export default {
    setup() {
      const tasks = ref([]);

      const addTasks = () => {
        tasks.value.push({
          title: "", 
          text: "", 
          input: "", 
          output: "", 
          time_limit: "1"
        })
      }

      addTasks();
      const index = ref(0);

      const removeTask = () => {
        if(tasks.value.length === 1) return;
        tasks.value.splice(index.value, 1);
      }

      const fileUpload = () => {
        
        let text = `const tasks = [`;

        tasks.value.forEach((el, index) => {
          text += `\n{`
          for(const key in el) {
            text += `"${key}":"${el[key]}"`
            if(key !== 'time_limit') {
              text += `,\n`
            }
          }
          text += `}`
          if(index !== tasks.value.length - 1) {
            text += `,\n`
          }
          
        })


        text += `]; \n export default tasks;`

        const blob = new Blob([text], { type: 'text/javascript' });

        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url;
        a.download = 'tasks.js';  
        document.body.appendChild(a);
        a.click();

        URL.revokeObjectURL(url);
      }
      return {
        tasks, index, fileUpload, addTasks, removeTask
      }
    }
  }
</script>

<style scoped>
  .container {
    max-width: 900px;
    margin: 0 auto;
  }
  .wrapper {
    width: 900px;
    padding: 10px 20px;
    border: 1px solid gray;
    border-radius: 20px;
    box-shadow: 0 0 1px 1px black;
    -webkit-box-sizing: border-box; /* Safari/Chrome, other WebKit */
    -moz-box-sizing: border-box;    /* Firefox, other Gecko */
    box-sizing: border-box; 
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-bottom: 10px;
  }
  .counter-tasks {
    margin: 0;
    padding: 0;
  }
  input, textarea {
    max-width: 900px;
  }
  input {
    height: 2em;
  }
  textarea {
    height: 8em;
  } 
  .button-wrapper {
    display: flex;
    justify-content: center;
    gap: 20px;
  }
  .input-time {
    display: flex;
    justify-content: start;
    gap: 10px;
    align-items: center;
  }
  .input-time input {
    width: 20px;
  }
</style>
