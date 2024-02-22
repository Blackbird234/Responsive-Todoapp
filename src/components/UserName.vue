<template>
  <div class="popup" v-if="showModal">
    <div class="popup-content">
      <label for="NameInput"> Bitte teile deinen Namen mit</label>
      <input type="text" name="NameInput" v-model="NameInput" required @keyup.enter="closeModal">
      <button type="button" class="close-button state" @click="closeModal"> <span>Absenden</span></button>
    </div>
  </div>
</template>
  
<script>
  export default {
    props: {
      showModal: Boolean,
      },

    data () {
      return {
        NameInput:''
      };
    },
    methods: {
      closeModal() {
        this.$emit('closeModal',this.NameInput);
      }
    }
  }

</script>
  
<style>
</style>
  
