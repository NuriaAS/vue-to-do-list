<template>
    <section class="tasks">
        <ul class="tasks__list">
            <li class="tasks__list__item">
                <section class="tasks__list__item__title">
                    <h4>List item to show</h4>
                    <h4>X</h4>
                </section>
                <section class="tasks__list__item__info">
                    <p class="creation">Created: 22/03/21</p>
                    <p class="when">Do on: 30/03/21</p>
                    <p class="task-priority">Priority: Higth</p>
                </section>
            </li>
        </ul>
        <h1>LIST</h1>
    </section>
</template>

<script>
export default {
  name: 'List',
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: block;
  border-radius: 10px 10px 10px 10px;
  background-color: #ece7e7;
}
/* .tasks_list {
} */
.tasks__list__item section {
  display: flex;
  padding: 0rem 1rem;
}
.tasks__list__item__title {
  /* width: 100%; */
  justify-content: space-between;
  border-bottom: 1px solid rgba(92, 95, 97, 0.3);
  
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
