<template>
  <section class="tasks">
    <ul class="tasks__list">
      <list-item 
        v-for="(item) in tasks" 
        :key="item.taskCreationDateSort"
        :taskIsDone="item.taskIsDone"
        :taskName="item.taskName"
        :taskId="item.taskCreationDateSort"
        :taskCreationDate="item.taskCreationDateFormated"
        :taskExecutionDate="item.taskExecutionDateFormated"
        :taskPriority="formatPriorityValue(item.taskPriority)"
        @clickEvent="deleteHandler"
        @changeEvent="checkHandler"
      />
    </ul>
  </section>
</template>

<script>
import { EventBus } from '@/event-bus.js';
import ListItem from './ListItem.vue';
export default {
  name: 'List',
  components: {
    ListItem,
  },
  data() {
    return {
      tasks: [],
      order: '',
    }
  },
  mounted() {
    this.retrieveTasks();
    EventBus.$on('task', (item) => {
      this.tasks.push(item);
      this.persistTasks();     
    });
    EventBus.$on('order', (show) => {
      this.order = show;
    });
  },
  methods: {
    deleteHandler(value) {
      let newTasks = this.tasks.filter(task => task.taskCreationDateSort !== value);
      this.tasks = newTasks;
      this.persistTasks();
    },
    toggleDoneState(item, id) {
      if (item.taskCreationDateSort === id) {
        item.taskIsDone = !item.taskIsDone;
        return item;
      } else {
        return item;
      }
    },
    checkHandler(value) {
      let newTasks = this.tasks.map(task => this.toggleDoneState(task, value));
      this.tasks = newTasks;
      this.persistTasks();      
    },
    formatPriorityValue(value) {
      const priorityMap = {
        1: 'High',
        2: 'Medium',
        3: 'Low'
      }
      return priorityMap[value];
    },
    persistTasks() {
      const parsed = JSON.stringify(this.tasks);
      localStorage.setItem('tasks', parsed);
    },
    retrieveTasks() {
      if(localStorage.getItem('tasks')) {
        const storedData = JSON.parse(localStorage.getItem('tasks'));
        this.tasks = storedData;
      }
    }
  },
  watch: {
    order(val) {
      let newTasksList = [...this.tasks];
      if(val === 'highPriority' ) {
          newTasksList.sort((a, b) => a.taskPriority - b.taskPriority);
     
      } else if(val === 'lowPriority') {
        newTasksList.sort((a, b) => b.taskPriority - a.taskPriority);
      } else if(val === 'alphabetical') {
        newTasksList.sort(function(a,b){
          return a.taskName.localeCompare(b.taskName);
        })
      } else if(val === 'alphabeticalReverse') {
        newTasksList.sort(function(a,b){
          return b.taskName.localeCompare(a.taskName);
        })
      } else if (val === 'when') {
        newTasksList.sort((a, b) => b.taskDateExecutionSort - a.taskDateExecutionSort);
      } else if (val === 'whenReverse') {
        newTasksList.sort((a, b) => a.taskDateExecutionSort - b.taskDateExecutionSort);
      }  else if (val === 'createdReverse') {
        newTasksList.sort((a, b) => b.taskCreationDateSort - a.taskCreationDateSort);
        
      } else if (val === 'created') {
         newTasksList.sort((a, b) => a.taskCreationDateSort - b.taskCreationDateSort);
      }
      this.tasks = newTasksList;
    },
  }
}


</script>

<style scoped>
.tasks__list {
  list-style-type: none;
  padding: 0;
}

@media only screen and (min-width: 767px) {
  .tasks {
    width: 80%;
    margin: auto;
  }
}
</style>
