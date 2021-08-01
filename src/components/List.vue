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
        1: 'Low',
        2: 'Medium',
        3: 'High'
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
    },
    sortList(conditionString) {
      if(!this.tasks.length) return;

      let newTasksList = [...this.tasks];
      const taskKey = conditionString.replace("Reverse", "");
      const taskValid = newTasksList.find(task => task[taskKey]);
      const isReverse = conditionString.includes('Reverse');
      const typeofData = typeof taskValid[taskKey];
      
      if (typeofData === 'string') {
        if(isReverse) {
          newTasksList.sort(function(a,b){
            return b[taskKey].localeCompare(a[taskKey]);
          })
        } else {
          newTasksList.sort(function(a,b){
            return a[taskKey].localeCompare(b[taskKey]);
          })
        } 
      } else if (typeofData === 'number') {
        if (isReverse) {
          newTasksList.sort((a, b) => a[taskKey] - b[taskKey]);
        } else {
          newTasksList.sort((a, b) => b[taskKey] - a[taskKey]);
        }
      } else {
        return;
      }

      this.tasks = newTasksList;
    }
  },
  watch: {
    order(val) {
     this.sortList(val);
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
