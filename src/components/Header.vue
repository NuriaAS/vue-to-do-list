<template>
  <div class="header">
    <h1>{{ msg }}</h1>
    <form class="header__form">
      
      <label for="task" class="header__form__label-task">
          Task
          <input type="text" placeholder="Write a task" name="task" class="header__form__label-task__input">
      </label>
      <div class="header__form__selectors">
        <label for="priority" class="header__form__selectors__label">
          Priority
          <select name="priority" id="" class="header__form__selectors__label__select">
              <option value="higth" selected>Higth</option>
              <option value="medium">Medium</option>
              <option value="low">Low</option>
          </select>
        </label>
        <label for="time" class="header__form__selectors__label">
          When
          <input type="date" name="time" id=""  class="time">
        </label>
        <button class="header__form__selectors__submit-btn">
          Save
        </button>
      </div>
      </form>
      <section class="header__order">
        <label for="" class="header__order__label">
          Order list by:
          <select name="order" id="" class="header__order__label__list-order">
              <option value="priority" selected>Higth to low priority</option>
              <option value="priorityReverse">Low to higth priority</option>
              <option value="created">Last to first added</option>
              <option value="createdReverse">First to last added</option>
              <option value="when">First to last to do</option>
              <option value="whenReverse">Last to first to do</option>
              <option value="alphabetical">From A to Z</option>
              <option value="alphabeticalReverse">From Z to A</option>
          </select>
      </label>
      
      <pre>{{ $data }}</pre>
      
    </section>
  </div>
</template>

<script>
export default {
  name: 'ToDoList',
  props: {
    msg: String
  }
  
}
</script>
  
<!-- Add "scoped" attribute to limit CSS to this component only -->
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
  .header__form__label-task__input, label .header__form__selectors__label__select, label .time {
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
  input, select {
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
     .header__form, .header__order {
        width: 60%;
        margin: auto;
      }
  }
</style>
