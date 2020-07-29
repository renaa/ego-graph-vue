<template>
  <div class="cube">

      <input type="text" v-model="query" @keyup.enter="getData" placeholder="search for something"/>
      <button @click="getData">search</button>

  </div>
</template>

<script>
const axios = require("axios")

export default {
  data() {
    return {
      query: "",
      searching: false,
    }
  },
  methods: {
    getData() {
      this.searching = true,
      this.$emit("searching")
      var url = "https://uniquefunctionname.azurewebsites.net/api/EgoGraph?name=" + this.query
      // var url = "http://localhost:7071/api/EgoGraph?name=" + this.query // local function
      axios
        .get(url)
        .then(response => {
          if (response.status === 200) {
            this.$emit("result", response.data)
          } else {
            console.log("server problems:", { response })
          }
        })
        .catch(error => console.log(error))
      this.searching = false
    },
  },
}
</script>

<style scoped>
.cube{
    background-color: white;
  height: 50px;
  width: 300px;
  box-shadow: 15px 15px 50px rgba(0,0,0,0.5);
  border-radius: 5px;
  margin: auto;
  display: flex;
  padding: 0 8px 0 15px;
  justify-content: space-between;
  align-items: center;
}
input {
  border: none;
  outline: none;
  height: 90%;
  font-size: 1.1em;
}
button{
  border: none;
  color: #8cffc9;
  background-color: #073b4c;
  height: 25px;
  outline: none;
  border-radius: 3px;
  height: 70%;
  
}
</style>
