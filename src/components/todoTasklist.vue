<template>

  <div class="todo" v-bind:class="{ greyOverlay: isGreyOverlay }">
    <h2 class="tasklist">Task list</h2>

    <p class="doNotHaveTasks" v-if="remainingTodos.length == 0 && completedTodos.length == 0">You don't have tasks to do.</p>
    <p
            class="imageAllDone"
            v-if="remainingTodos.length == 0 && completedTodos.length !== 0"
    >
      <img
              src="../assets/fireworks32.png"
              alt=""
      >
    </p>
    <p class="allDone" v-if="remainingTodos.length == 0 && completedTodos.length !== 0">All done!</p>

    <todoRemainingTodos
      @check-task="sortTodos" :remaining-todos="remainingTodos"
      @delete-todo="deleteTodoWindow" :is-disable-delete-btn="isDisableDeleteBtn" :is-disabled-checkbox="isDisabledCheckbox"
      @edit-todo-dbl-click="editTodoDblClickExact" :is-content-editable="isContentEditable" :pass-id="passId"
      @disable-content-editable="changeIsContentEditable"
      @edit-exact-todo="editExactTodo">
    </todoRemainingTodos>

    <todoInput
    @add-task="addTodo" :is-disabled-input="isDisabledInput">
    </todoInput>

    <todoPercentageDone
      :remaining-todos="remainingTodos"
      :completed-todos="completedTodos"
      :percentage-calc-color="percentageCalcColor"
      :is-line-through-show="isLineThroughShow"
    ></todoPercentageDone>

    <todoCompletedTodos
    @uncheck-task="unSortTodos" :completed-todos="completedTodos"
    @delete-todo="deleteTodoWindow" :is-disable-delete-btn="isDisableDeleteBtn" :is-disabled-checkbox="isDisabledCheckbox">
    </todoCompletedTodos>

    <todoDeleteWindow v-if="yesNoShow"
    :delete-input="deleteInput"
    @cancel-delete-window="buttonNo"
    @delete-todo="deleteExactTodo"
    :change-is-disable-delete-btn="changeIsContentEditable">
    </todoDeleteWindow>
  </div>


</template>

<script>
  import todoRemainingTodos from "./todoRemainingTodos";
  import todoInput from "./todoInput";
  import todoPercentageDone from "./todoPercentageDone";
  import todoCompletedTodos from "./todoCompletedTodos";
  import todoDeleteWindow from "./todoDeleteWindow";

  export default {

    components:{
      "todoRemainingTodos": todoRemainingTodos,
      "todoInput": todoInput,
      "todoPercentageDone": todoPercentageDone,
      "todoCompletedTodos": todoCompletedTodos,
      "todoDeleteWindow": todoDeleteWindow

    },

    data () {
      return {
        todoArrayShow: [],
        yesNoShow: false,
        isDisabledCheckbox: false,
        isDisableDeleteBtn: false,
        isDisabledInput: false,
        isContentEditable: false,
        isGreyOverlay: false,
        isLineThroughShow: false,

        deleteInput: "",
        passIdDeleteExactTodo: "",
        passId: null

      }
    },

    watch: {

      todoArrayShow: {
        handler: function (newValue) {

          localStorage.setItem("todoArrayShow", JSON.stringify(newValue));
        },
        deep: true
      }
    },

    computed: {
      remainingTodos: function () {
        return this.todoArrayShow.filter(function (x) {
          return !x.show;
        })
      },

      completedTodos: function () {
        return this.todoArrayShow.filter(function (x) {
          return x.show
        })
      },

      percentageCalcColor() {
        return Math.round(this.completedTodos.length / (this.remainingTodos.length + this.completedTodos.length) * 100);
      }

    },

    methods: {
      addTodo(task){
        this.todoArrayShow.push(task);

        const arrayStringify = JSON.stringify(this.todoArrayShow);

        const encode = btoa(arrayStringify);

        const myUrl = new URL ("2020-september-todolist-cli.vercel.app");
        myUrl.hash = encode;
        window.location = myUrl;

        localStorage.setItem("todoArrayShow", JSON.stringify(this.todoArrayShow));
      },
      sortTodos(item){
        const findId = this.todoArrayShow.find(el => el.id === item.id);
        findId.show = true;
      },
      unSortTodos(item){
        const findId = this.todoArrayShow.find(el => el.id === item.id);
        findId.show = false;
      },
      deleteTodoWindow(item){
        this.yesNoShow = true;
        this.isGreyOverlay = true;
        this.isDisabledCheckbox = true;
        this.isDisableDeleteBtn = true;
        this.isLineThroughShow = true;
        this.isDisabledInput = true;
        this.deleteInput = this.todoArrayShow.find(i => i.id === item).text;
        this.passIdDeleteExactTodo = item
      },

      buttonNo(){
        this.yesNoShow = false;
        this.isGreyOverlay = false;
        this.isDisabledCheckbox = false;
        this.isDisableDeleteBtn = false;
        this.isLineThroughShow = false;
        this.isDisabledInput = false;
      },

      deleteExactTodo(){
        this.todoArrayShow = this.todoArrayShow.filter((item) => item.id !== this.passIdDeleteExactTodo);

        localStorage.setItem("todoArrayShow", JSON.stringify(this.todoArrayShow));

        this.yesNoShow = false
        this.isGreyOverlay = false;
        this.isDisabledCheckbox = false;
        this.isDisableDeleteBtn = false;
        this.isLineThroughShow = false;
        this.isDisabledInput = false;
      },
      editTodoDblClickExact(id){
        this.changeIsContentEditable();
        this.passId = id;
      },
      changeIsContentEditable(){
        this.isContentEditable = !this.isContentEditable;
      },

      editExactTodo(e){
        this.x = e.target.textContent

        const findExactId = this.todoArrayShow.find(i => i.id === this.passId)
        findExactId.text = this.x

        localStorage.setItem("todoArrayShow", JSON.stringify(this.todoArrayShow));

        this.changeIsContentEditable()

        // this.$emit("disable-content-editable")
      }
    },
    created(){
      const localStorageItems = JSON.parse(localStorage.getItem("todoArrayShow"));

      if (localStorageItems) {
          this.todoArrayShow = localStorageItems;
      }

      localStorage.setItem("todoArrayShow", JSON.stringify(this.todoArrayShow));
    }

  }
</script>

<style scoped>

  .todo{
    padding: 20px;
    position: absolute;
    top: 80px;
    /*background-color: #fafcfb;*/
    background-color: rgba(250,252,252, 1);
    /*height: 60%;*/
    width: 60%;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  }

  .tasklist{
    text-align: center;
    padding-bottom: 35px;
  }

  .doNotHaveTasks {
    text-align: center;
    color: #696969;
    font-weight: bold;
    margin: 5px 0;
  }

  .imageAllDone{
    text-align: center;
    margin: 5px 0;
  }

  .allDone{
    text-align: center;
    color: #696969;
    font-weight: bold;
    margin: 5px 0;
  }

  .greyOverlay {
    background-color: rgba(255,255,255, 0.7);
  }

</style>
