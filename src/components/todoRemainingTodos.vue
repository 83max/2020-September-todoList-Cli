<template>

  <ul class="ulRemaining">
    <li class="liRemaining" v-for="item in remainingTodos" :key="item.id">
      <span class="checkboxContainer"><input type="checkbox" v-bind:id="item.id" class="checkboxClass" v-on:click="checkTask(item)">
        <label v-bind:class="{ disabledCheckbox: isDisabledCheckbox }" class="checkboxLabel" v-bind:for="item.id"> </label> </span>
      <label v-bind:id="item.id"
             class="labelRemainingTodo"
             ref="remainingTodoRef"
             v-bind:contenteditable="isContentEditable"
             v-on:dblclick="editTodoDblClick(item.id)"
             v-on:keydown.prevent.enter="editTextInLi">
        {{ item.text }}
      </label>
      <span
        v-bind:class=" {disableDeleteBtn: isDisableDeleteBtn} "
        class="fa fa-trash-o" aria-hidden="true"
        v-on:click="deleteTodo(item.id)"
      ></span>
    </li>
  </ul>

</template>

<script>

  export default {
    props: {
      remainingTodos: {
        type: Array,
        required: false
      },
      isContentEditable: {
        type: Boolean,
        required: true
      },
      passId: {
        type: Number,
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

    data () {
      return {
        // isDisableDeleteBtn: false,
        yesNoShow: false,
        x: "",
        y: null,


      }
    },
    methods: {
      checkTask(item){
        this.$emit("check-task", item)
      },
      deleteTodo(item){
        this.$emit("delete-todo", item)
        this.$emit("is-disable-delete-btn")
      },
      editTodoDblClick(item){
        this.$emit("edit-todo-dbl-click", item)
      },
      editTextInLi(item){
        this.$emit("edit-exact-todo", item)
        // this.x = e.target.textContent
        // console.log(e.target)
        // console.log(this.x)
        // console.log(this.passId)
        //
        // this.$emit("disable-content-editable")

        // const aa = this.todoArrayShow.find(i => i.id === this.passId)
      }
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
    /*left: 5px;*/
    content: "";
    background-color: #eee0;
    border: 2px solid #aabfb0;
    display: inline-block;
    width: 24px;
    height: 24px;
    border-radius: 50%;
  }

  .checkboxLabel:hover{
    background-color:#e1e8e3;
  }

  .checkboxClass{
    position: absolute;
    left: -9999px;
  }

  .disableDeleteBtn{
    pointer-events: none;
  }

  .disabledCheckbox{
    pointer-events: none;
  }

</style>
