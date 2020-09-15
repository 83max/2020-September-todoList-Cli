<template>
<div>
<ul>

    <li
      v-for="item in hideTodos" :key="item.id">
      <span class="checkboxContainer"><input v-bind:id="item.id" class="checkboxClass" type="checkbox" v-on:click="unCheckTask(item)">
          <label v-bind:class="{ disabledCheckbox: isDisabledCheckbox }" class="checkboxLabel" v-bind:for="item.id"> </label> </span>
      <label class="labelCompletedTodo" v-bind:style="lineThrough"> {{ item.text }} </label>
      <span class="fa fa-trash-o" aria-hidden="true"
            v-bind:class=" {disableDeleteBtn: isDisableDeleteBtn} "

            v-on:click="deleteTodo(item.id)"
      ></span>
    </li>
</ul>

  <p v-if="showMoreCondition" v-on:click="showMoreThanFiveTodos" class="showMore">Show more...</p>

</div>
</template>

<script>


  export default {
    props: {
      completedTodos: {
        type: Array,
        required: false
      },
      isDisableDeleteBtn: {
        type: Boolean,
        required: true
      },
      isDisabledCheckbox: {
        type: Boolean,
        required: true
      },

    },
    data() {
      return {
        lineThrough: {
          textDecoration: "line-through"
        },
        hideShowMore: true

      }
    },

    computed:{
      hideTodos(){
        if (this.hideShowMore) {
        return this.completedTodos.slice(0, 5)
        } else {
        return this.completedTodos;
        }
      },
      showMoreCondition(){
        return this.completedTodos.length > 5 && this.hideShowMore;
      }
    },

    methods: {
      unCheckTask(item){
        this.$emit("uncheck-task", item)
      },
      deleteTodo(item){
        this.$emit("delete-todo", item)
      },
      showMoreThanFiveTodos(){
        this.hideShowMore = false;
      },

    }
  }
</script>

<style scoped>

  ul{
    list-style: none;
    margin: 0;
    padding: 0;
    padding-left: 20px;
    padding-right: 20px;
  }

  li{
    display: flex;
    justify-content: space-between;
    padding-bottom: 10px;
  }

  .checkboxLabel{
    position: relative;
    cursor: pointer;
  }

  .checkboxLabel{
    position: absolute;
    /*left: -24px;*/
    content: "";
    background-color: #aabfb0;
    border: 2px solid #aabfb0;
    display: inline-block;
    width: 24px;
    height: 24px;
    border-radius: 50%;
  }

  .checkboxClass{
    position: absolute;
    left: -9999px;
  }

  .showMore{
    cursor: pointer;
    text-align: center;
    font-size: small;
  }

  .showMore:hover{
    color: #aabfb0;
  }

  .disableDeleteBtn{
    pointer-events: none;
  }

  .disabledCheckbox{
    pointer-events: none;
  }

</style>
