<template>
<div>
  <input type="text" v-model="query" />
  <button @click="getData()">search</button>
</div>
</template>

<script>
const axios = require("axios")

export default {
  data() {
    return {
      query: ''
    }
  },
  methods: {
    getData() {
      var url = "https://uniquefunctionname.azurewebsites.net/api/EgoGraph?name=" + this.query
      axios
        .get(url)
        .then(response => {
          if (response.status === 200){
            this.$emit('myEmit', response.data)
          } 
          else {
            console.log('server problems:', {response})
          }
        })
        .catch(error => console.log(error))
    },
  },
}
</script>

<style></style>
