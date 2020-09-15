<template>

  <div class="deleteYesNo">
    <p>Are you sure you want to delete "{{ deleteInput }}"?</p>
    <span>
            <button class="btnYes" v-on:click="yesBtn">Yes</button>
            <span></span>
            <button class="btnNo" v-on:click="noBtn">No</button>
        </span>
  </div>

</template>

<script>


  export default {
    props: {
      deleteInput: {
        type: String,
        required: false
      },
    },
    data () {
      return {
        yesNoShow: false,
        isGreyOverlay: false
      }
    },
    methods: {
      noBtn(){
        this.$emit("cancel-delete-window")
        // this.$emit("is-grey-overlay")
      },
      yesBtn(){
        this.$emit("delete-todo")
      }
    }
  }
</script>

<style scoped>

  .deleteYesNo {
    position: absolute;
    z-index: 1;
    top: 100px;
    left: 250px;
    text-align: center;
    padding: 20px;
    background: #fafcfb;
    width: 40%;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  }

  .btnYes {
    height: 30px;
    width: 50px;
    margin-right: 10px;

  }

  .btnNo {
    height: 30px;
    width: 50px;
    margin-left: 10px;
  }

</style>
