<template>
    <li class="tasks__list__item">
        <section class="tasks__list__item__title">
          <input type="checkbox" v-model="isDone" @change="changeHandler"> 
          <h4 :class="taskFinished">{{ taskName }}</h4>
          <button @click="clickHandler">X</button>
        </section>
        <section class="task__list__item__info">
          <p class="creation">Created: {{ taskCreationDate }}</p>
          <p class="when">Do on: {{ taskExecutionDate }}</p>
          <p class="task-priority">Priority: {{ taskPriority }}</p>
        </section>
      </li>
</template>
<script>

export default ({
    name: 'ListItem',
    data() {
      return {
        isDone: this.taskIsDone,
      }
    },
    props: {
        taskName: {
          type: String,
          required: true
        },
        taskCreationDate: {
          type: String,
          required: true
        },
        taskExecutionDate: {
          type: String,
          required: true
        },
        taskPriority: {
          type: String,
          required: true
        },
        taskId: {
          type: Number,
          required: true
        },
        taskIsDone: {
          type: Boolean,
          required: true,
        }
    },
    methods: {
      clickHandler() {
        this.$emit("clickEvent", this.taskId);
      },
      changeHandler() {
        this.$emit("changeEvent", this.taskId);
      }
    },
    computed: {
      taskFinished() {
        return this.isDone ? 'task-done' : null;
      }
    }
})
</script>
<style scoped>
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
.task-done {
  text-decoration: line-through #53a153;
  color: #53a153;
}
</style>