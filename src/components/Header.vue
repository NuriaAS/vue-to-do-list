<template>
  <div class="header">
    <h1>{{ msg }}</h1>
    <form class="header__form" v-on:submit.prevent="onSubmit">
      <label for="task" class="header__form__label-task">
          Task
          <input type="text" placeholder="Write a task" name="task" class="header__form__label-task__input" v-model="newTaskName" required>
      </label>
      <div class="header__form__selectors">
        <label for="priority" class="header__form__selectors__label">
          Priority
          <select name="priority" id="" class="header__form__selectors__label__select" v-model="newTaskPriority" required>
            <option value="1" selected>High</option>
            <option value="2">Medium</option>
            <option value="3">Low</option>
          </select>
        </label>
        <label for="time" class="header__form__selectors__label">
          When
          <input type="date" name="time" id="" class="time" v-model="newTaskDate" required>
        </label>
        <button class="header__form__selectors__submit-btn" @click="submitHandler">
          Save
        </button>
      </div>
    </form>
    <section class="header__order">
      <label for="" class="header__order__label">
        Order list by:
        <select name="order" class="header__order__label__list-order" v-model="order" @change="onChange()">
          <option value="created" selected>Last to first added</option>
          <option value="createdReverse">First to last added</option>
          <option value="highPriority">High to low priority</option>
          <option value="lowPriority">Low to high priority</option>
          <option value="when">First to last to do</option>
          <option value="whenReverse">Last to first to do</option>
          <option value="alphabetical">From A to Z</option>
          <option value="alphabeticalReverse">From Z to A</option>
        </select>
      </label>
    </section>
  </div>
</template>

<script>
import { EventBus } from '@/event-bus.js';
export default {
  name: 'ToDoList',
  props: {
    msg: String
  },
  data() {
    return {
      newTaskName: "",
      newTaskPriority: "1",
      newTaskDate: "",
      newtaskCreation: "",
      taskItem: {},
      order: "created",
    }
  },
  methods: {
    setListItem() {
      this.taskItem = {
        taskName: this.newTaskName,
        taskPriority: this.newTaskPriority,
        taskDate: new Date(this.newTaskDate),
        taskCreated: this.newtaskCreation
      } 
    },
    resetModel() {
      this.newTaskName = "";
      this.newTaskPriority = "1";
      this.newTaskDate = "";
    },
    submitHandler() {
      if(this.newTaskName && this.newTaskDate) {
        this.newtaskCreation = new Date();
        this.setListItem();
        EventBus.$emit('task', this.taskItem);
      }
      this.resetModel();
    },
    onChange() {
      EventBus.$emit('order', this.order);
    }
  }
}
</script>

<style scoped>
  .header {
    padding-bottom: 2rem;
  }
  .header__form {
    border-radius: 10px 10px 0 0;
    padding-bottom: 2rem;
  }
  .header__form input  {
    display: block;
  }
  .header__form__label-task__input,
  label .header__form__selectors__label__select, 
  label .time {
    width: 100%;
  }
  .header__form__label-task__input {
    margin-bottom: 1rem ;
  }
  label {
    font-size: 0.9rem;
    font-weight: 600;
    text-align: left;
  }
  .header__form__label-task {
    display: block;
    width: 100%;
    text-align: left;
  }
  .header__form__selectors__submit-btn {
    border: none;
    background-color: rosybrown;
    color: white;
    font-weight: 600;
    border-radius: 3px;
    flex-basis: 15%;
    height: 100%;
    padding: 0.45rem;
    cursor: pointer;
  }
  .header__form__selectors__submit-btn:hover {
    border: 1px solid rosybrown;
    background-color: transparent;
    color: rosybrown;
  }
  input, 
  select {
    border-radius: 5px;
    border: 1px solid #997676;
    height: 1.5rem;
  }
  .header__form__selectors {
    display: flex;
    width: 100%;
    justify-content: space-evenly;
    align-items: flex-end;
  }
  .header__order {
    border-radius: 0 0 10px 10px;
  }
  @media only screen and (min-width: 767px) {
    .header__form, 
    .header__order {
      width: 60%;
      margin: auto;
    }
  }
</style>
