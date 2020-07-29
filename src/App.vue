<template>
  <div id="app">
    <h1 class="egograph">
      Egograph
      <div class="tooltiptext">
        An ego graph is a great way to get to know a new thing. It is graphs where we can see connected terms together and how they compare. This is done by recursively googling whatever you serch for, and adding vs (versus). fex. "puppies vs" and seeing the search results that come up, then doing the same to them. 
      <a class="egograph" href="https://medium.com/applied-data-science/the-google-vs-trick-618c8fd5359f">here is an article about it</a>
        <br> The thicker the line, the stronger the connection.
        </div>
      </h1>
    <Query @result="onDataFromServer" />
    <div class="d3Wrapper">

    <d3-network class="d3-network" :net-nodes="nodes" :net-links="links" :options="options" />
    </div>
  </div>
</template>

<script>
import Query from "./components/Query"
import D3Network from "vue-d3-network"

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
      options: {
        canvas: false,
        force: 3000,
        nodeLabels: true,
        // nodeSize: 20,
        fontSize: 20,
        // linkWidth: 1,
      },
    }
  },
  methods: {
    onDataFromServer(value) {
      if (value.Item1.length === 0) {
        console.log("no result from server")
      } else {
        this.nodes = value.Item1.map(x => {
          
          return {
            ...x,
            // _color: "gray",
            _size: x === value.Item1[0] ? 35 : 20,
             _labelClass: x === value.Item1[0] ? 'rootNodeLabel d3Label' : 'nodeLabel d3Label',

          }
        })
        this.links = value.Item2.map(x => {
          //x.name is strength of connection
          let colorspectrum = '#073b4c'
          let widths = [1, 3, 5, 7, 6]
          let color = colorspectrum
          let width = widths[0]

          if (x.name > 20) {
            color = colorspectrum
            width = widths[4]
          } else if (x.name > 15) {
            color = colorspectrum
            width = widths[3]
          } else if (x.name > 10) {
            color = colorspectrum
            width = widths[2]
          } else if (x.name > 5) {
            color = colorspectrum
            width = widths[1]
          }

          return {
            ...x,
            // _color: color,
            _svgAttrs: {
              stroke: color,
              "stroke-width": width,
            },
          }
        })
      }
      console.log(
        value.Item2.map(x => {
          return {
            ...x,
            // _color: "blue",
          }
        })
      )
    },
  },
}
</script>

<style lang="scss">
body{
background: rgb(239,71,111);
background: linear-gradient(351deg, rgba(239,71,111,1) 0%, rgba(255,209,102,1) 95%);
margin: 0;
}
a {
  text-decoration: none;
  color: #8cffc9 !important;
  font-size: 1rem !important;
}
.egograph{
  position: relative;
  display: inline-block;
  color: white;
  margin-top: 0;
  font-size: 2.3rem;
  text-shadow: 0px 10px 20px rgba(0, 0, 0, 0.7);
  width: 180;
}
.egograph:hover{
  color: #076460;

}
.tooltiptext{
  border-radius: 5px;
  padding: 15px;
  margin-top: 12px ;
  visibility: hidden;
  font-size: 1rem;
  position: absolute;
  left: -97px;
  width: 360px;
  font-weight: 200;
  
  background-color: #073b4cbf;
  margin: 0;
  text-align: center;
}
.egograph:hover .tooltiptext{
  visibility: visible;
  color: white;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #076460;
  margin-top: 30px;
  overflow: hidden;
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

.d3-network {
  height: 91vh;
  width: 100%;
  
}
.node{
  fill: #073b4c;
  stroke: #8cffc9;
  // stroke:#06d6a0;
  stroke-width: 4px;
}
.d3Label{
  fill: white;
  cursor: unset;
  user-select: none;


}
.nodeLabel{
  font-size: 20px;
  
}
.rootNodeLabel{
  font-size: 30px;
}

.d3Wrapper{
  z-index: 3;
}
</style>
