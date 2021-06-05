<template>
  <section class="tasks">
    <ul class="tasks__list">
      <li class="tasks__list__item" v-for="(item, index) in tasks" v-bind:key="index" >
        <section class="tasks__list__item__title">
          <h4>{{ item.task }}</h4>
          <a @click="deleteHandler">X</a>
        </section>
        <section class="task__list__item__info">
          <p class="creation">Created: 22/03/21</p>
          <p class="when">Do on: {{ item.date }}</p>
          <p class="task-priority">Priority: {{ formatPriorityValue(item.priority) }}</p>
        </section>
      </li>
    </ul>
  </section>
</template>

<script>
import { EventBus } from '@/event-bus.js';
export default {
  name: 'List',
  data() {
    return {
      tasks: [],
       order: '',
    }
  },
  created() {
    EventBus.$on('task', (item) => {
      this.tasks.push(item)
    });
    EventBus.$on('order', (show) => {
      this.order = show;
    })
  },
  methods: {
    deleteHandler(index) {
      this.tasks.splice(index, 1);
    },
    formatPriorityValue(value) {
      const priorityMap = {
        1: 'High',
        2: 'Medium',
        3: 'Low'
      }
      return priorityMap[value];
    }
  },
  watch: {
    order(val) {
      let newTasksList = [...this.tasks];

      if(val === 'highPriority') {
        newTasksList.sort(function(a,b) {
          if(a.priority > b.priority) {
            return 1;
          } else {
            return -1;
          }
        })
      }

      this.tasks = newTasksList;
    }
  },
}


</script>

<style scoped>
.tasks__list {
  list-style-type: none;
  padding: 0;
}
.tasks__list__item {
  display: block;
  border-radius: 10px;
  background-color: #ece7e7;
  margin-bottom: 1rem;
}
.tasks__list__item section {
  display: flex;
  padding: 0 1rem;
}
.tasks__list__item__title {
  justify-content: space-between;
  border-bottom: 1px solid rgba(92, 95, 97, 0.3);
}
.tasks__list__item__title a {
  cursor: pointer;
  padding: 0.3rem;
  align-self: center;
  font-weight: 600;
}
.tasks__list__item__info p {
  margin-left: 1rem;
  font-size: 0.8rem;
  padding: 0.3rem;
  border: 1px solid #5c5f61;
  border-radius: 3px;
}
.tasks__list__item__info p:first-child {
  margin-left: 0;
}
@media only screen and (min-width: 767px) {
  .tasks {
    width: 80%;
    margin: auto;
  }
}
</style>
