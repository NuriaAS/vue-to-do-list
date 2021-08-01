<template>
  <div class="header">
    <h1>{{ msg }}</h1>
    <form class="header__form" @submit.prevent="submitHandler">
      <input-group 
        id="task"
        label="Task"
        type="text"
        @inputEvent="taskNameEventHandler"
        ref="inputName"
      />
      <input-group 
        id="inputDateName"
        label="labelDate"
        type="date"
        @inputEvent="dateEventHandler"
        ref="inputDate"
      />
      <div class="header__form__selectors">
        <label for="priority" class="header__form__selectors__label">
          Priority
          <select name="priority" id="" class="header__form__selectors__label__select" v-model="newTaskPriority">
            <option value="3">High</option>
            <option value="2">Medium</option>
            <option value="1">Low</option>
          </select>
        </label>
        <input type="submit" value="Save" class="header__form__selectors__submit-btn" :disabled="!validated"/>
      </div>
    </form>
    <section class="header__order">
      <label for="" class="header__order__label">
        Order list by:
        <select name="order" class="header__order__label__list-order" id="order" v-model="order" @change="onChange()">
          <option value="taskCreationDateSort">Last to first added</option>
          <option value="taskCreationDateSortReverse">First to last added</option>
          <option value="taskPriority">High to low priority</option>
          <option value="taskPriorityReverse">Low to high priority</option>
          <option value="taskDateExecutionSort">First to last to do</option>
          <option value="taskDateExecutionSortReverse">Last to first to do</option>
          <option value="taskName">From A to Z</option>
          <option value="taskNameReverse">From Z to A</option>
        </select>
      </label>
    </section>
  </div>
</template>

<script>
import { EventBus } from '@/event-bus.js';
import InputGroup from './InputGroup.vue';

export default {
  name: 'ToDoList',
  props: {
    msg: String
  },
  components: {
    InputGroup,
  },
  data() {
    return {
      newTaskName: "",
      newTaskPriority: "3",
      newTaskDate: "",
      taskItem: {},
      order: "taskCreationDateSort",
      validated: false,
    }
  },
  methods: {
    taskNameEventHandler(value) {
      this.newTaskName= value;
      this.validateForm()
    },
    dateEventHandler(value) {
      this.newTaskDate = value;
      this.validateForm()
    },
    setListItem() {
      this.taskItem = {
        taskName: this.newTaskName,
        taskPriority: this.newTaskPriority,
        taskDateExecutionSort: (new Date(this.newTaskDate)).getTime(),
        taskExecutionDateFormated: this.formatDate(new Date(this.newTaskDate)),
        taskCreationDateSort: (new Date()).getTime(),
        taskCreationDateFormated: this.formatDate(new Date()),
        taskIsDone: false,
      } 
    },
    submitHandler() {
      this.setListItem();
      EventBus.$emit('task', this.taskItem);
      this.resetModel();
      this.$refs.inputName.resetValue();
      this.$refs.inputDate.resetValue();
    },
    resetModel() {
      this.newTaskName = "";
      this.newTaskPriority = "3";
      this.newTaskDate = "";
      this.validated = false;
    },
    validateForm() {
      if(this.newTaskName && this.newTaskDate) {
        this.validated = true;
      } else {
        this.validated = false;
      }
    },
    onChange() {
      EventBus.$emit('order', this.order);
    },
    formatDate(date) {
      return `${date.getDate()}${'/'}${date.getMonth() + 1}${'/'}${date.getFullYear()}`;
    },
  },

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
  .header__form__selectors__submit-btn:disabled, .header__form__selectors__submit-btn:disabled:hover
  .header__form__selectors__submit-btn {
    border: none;
    color: white;
    font-weight: 600;
    border-radius: 3px;
    flex-basis: 15%;
    height: 100%;
    padding: 0.45rem;
  }
  .header__form__selectors__submit-btn:disabled {
    background-color: rgba(188, 143, 143, 0.5);
  }
  .header__form__selectors__submit-btn:disabled:hover {
    cursor: unset;
    border: none;
    background-color: rgba(188, 143, 143, 0.5);
    color: white;
  }
  .header__form__selectors__submit-btn {
    background-color: rgba(188, 143, 143, 1);
  }

  .header__form__selectors__submit-btn:hover {
    border: 1px solid rosybrown;
    background-color: transparent;
    color: rosybrown;
    cursor: pointer;
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
