<template>
  <div class="row my-5">

    <div class="col-2">
        <p class="text-end pt-2">Ricerca per: </p>
    </div>
    <div class="col-2">
        <select 
          class="form-select" 
          v-model="type"
        >
          <option value="author">Autore</option>
          <option value="title">Titolo</option>
        </select>
    </div>
    <div class="col-6">
      <input 
        type="text" 
        class="form-control" 
        placeholder="Cosa devi cercare?"
        v-model="textInput"
        @keyup.enter="checkValidSearch"
      >
    </div>
    
    <div class="col-1">
      <button 
        class="btn btn-info"
        @click="checkValidSearch"
      >Cerca</button>
    </div>
    <div class="col-1">
      <button 
        class="btn btn-warning"
        @click="resetSearch"
      >Reset</button>
    </div>

  </div>
</template>

<script>
export default {
  name: 'Search',
  data() {
    return {
      textInput: '',
      type: ''
    }
  },
  methods: {
    resetSearch() {

      this.textInput = '';
      this.type = '';

      this.$emit('reset');
    },
    checkValidSearch() {
      if (this.textInput !== '' && this.type !== '') {
        
        this.$emit('sendSearch', this.textInput, this.type);
      } else {
        alert('Attenzione!\nDevi complilare entrambi i campi')
      }
    }
  }
}
</script>

<style>

</style>