<template>
  <div id="app">
    <h1 class="egograph">
      Egograph <span class="ego-span"> ?</span>
      <div class="tooltiptext">
        An ego graph is a great way to get insight into ex. a technology (but it can be anything), and see connected search terms.
        <br />
        <br />
        This is done by recursively googling queries, and adding vs (versus), doing the
        same to the search results that come up, and building a graph from this.
        <br>
        <br>
        Example "puppies vs" gives us "puppies vs cats" then "cats vs" gives us "cats vs dogs"
        <br />
        <br />
        <a href="https://medium.com/applied-data-science/the-google-vs-trick-618c8fd5359f">
          Here is the article that gave me inspiration to make this.</a
        >
                <br>
        <br>
        The thicker the line, the more relevant the connection.
        <br>
        <br>
        This is a proof by concept. Some edge cases may not have been accounted for.
        <br>
        <br>
        <a class="octicon" href="https://github.com/renaa/ego-graph-vue">
          <svg
            class="octicon"
            height="32"
            viewBox="0 0 16 16"
            version="1.1"
            width="20"
            aria-hidden="true"
          >
            <path
              fill-rule="evenodd"
              d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0016 8c0-4.42-3.58-8-8-8z"
            ></path>
          </svg>
          repository
        </a>
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
        this.nodes = [
          {
            name: "NO RESULT",
            _labelClass: "nodeLabel, d3Label",
            _size: 20,
          },
        ]
        this.links = []
      } else {
        this.nodes = value.Item1.map(x => {
          return {
            ...x,
            _size: x === value.Item1[0] ? 35 : 20,
            _labelClass: x === value.Item1[0] ? "rootNodeLabel d3Label" : "nodeLabel d3Label",
          }
        })
        this.links = value.Item2.map(x => {
          //x.name is strength of connection
          let colorspectrum = "#073b4c"
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
            _svgAttrs: {
              stroke: color,
              "stroke-width": width,
            },
          }
        })
      }
      // console.log(value.Item1)
    },
  },
}
</script>

<style lang="scss">
body {
  background: rgb(239, 71, 111);
  background: linear-gradient(351deg, rgba(239, 71, 111, 1) 0%, rgba(255, 209, 102, 1) 95%);
  margin: 0;
}
a {
  text-decoration: none;
  color: #8cffc9;
  font-size: 1rem !important;
}
.egograph {
  position: relative;
  display: inline-block;
  color: white;
  margin-top: 0;
  font-size: 2.3rem;
  text-shadow: 0px 10px 20px rgba(0, 0, 0, 0.7);
  width: 180;
}
.egograph:hover {
  color: #076460;
}
.tooltiptext {
  border-radius: 5px;
  padding: 15px;
  margin-top: 12px;
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
.egograph:hover .tooltiptext {
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
  height: 100vh;
  width: 100%;
}
.node {
  fill: #073b4c;
  stroke: #8cffc9;
  // stroke:#06d6a0;
  stroke-width: 4px;
}
.d3Label {
  fill: white;
  cursor: unset;
  user-select: none;
}
.nodeLabel {
  font-size: 20px;
}
.rootNodeLabel {
  font-size: 30px;
}

.d3Wrapper {
  z-index: 3;
}
.octicon {
  color: #8cffc9;
  display: inline-block;
  vertical-align: middle;
  fill: currentColor;
  height: 10%;
}

.ego-span{
  vertical-align:top ;
  font-size: 0.5em;
  left: -10px;
  position: relative;
}
</style>
