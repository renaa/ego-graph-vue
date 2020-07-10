<template>
  <div id="app">

    <Query @result="onDataFromServer"/>

    <d3-network class="d3-network" :net-nodes="nodes" :net-links="links" :options="options" />

  </div>
</template>

<script>
import Query from "./components/Query"
import D3Network from 'vue-d3-network'

export default {
  name: "App",
  components: {
    Query,
    D3Network,

  },
  data: function() {
    return {
      response: "",
      
      isOpen: false,

      nodes: [],
      links: [],
      options:{
      canvas: false,
        force: 3000,
        nodeLabels: true,
        nodeSize: 20,
        fontSize: 20,

      },
    }
  },
  methods: {
    onDataFromServer(value) {
      console.log(value.Item1, value.Item1.length === 0)
      if(value.Item1.length === 0){
        console.log('no result from server')
      }
      else{
      this.nodes = value.Item1.map(x => {
        return {
          ...x,
          _color: 'gray'
        }
      })
      this.links = value.Item2.map(x => {
        return{
          ...x, 
          _color: 'gray'
        }
      });
      }
    },
    

  },
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  // display: flex;
  // flex-direction: column;
  // align-items: center;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease-out;
}

.d3-network{
  height: 100vh;
}
</style>
