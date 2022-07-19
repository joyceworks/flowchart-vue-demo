<template>
  <div id="app">
    <div>
      <button type="button"
              @click="$refs.chart.add({id: +new Date(), x: 10, y: 10, name: 'New', type: 'operation', approvers: []})">
        Add
      </button>
      <button type="button" @click="$refs.chart.remove()">
        Del
      </button>
      <button type="button" @click="$refs.chart.editCurrent()">
        Edit
      </button>
      <button type="button" @click="$refs.chart.save()">
        Save
      </button>
    </div>
    <flowchart :nodes="nodes" :connections="connections" @editnode="handleEditNode"
               @dblclick="handleDblClick" @editconnection="handleEditConnection"
               @save="handleChartSave" ref="chart" @render="handleRender">
    </flowchart>
  </div>
</template>

<script>
import Flowchart from "flowchart-vue";

export default {
  name: 'App',
  components: {
    Flowchart
  },
  data: function () {
    return {
      nodes: [
        {
          id: 1,
          x: 50,
          y: 220,
          name: "Start",
          type: "start",
          bodyBackgroundColor() {
            return 'green';
          },
          titleBackgroundColor() {
            return 'green';
          },
          borderColor() {
            return 'red';
          },
          textColor(isSelected) {
            return isSelected ? 'gray' : 'blue';
          }
        },
        {id: 2, x: 630, y: 220, name: "End", type: "end"},
        {
          id: 3,
          x: 340,
          y: 130,
          name: "Custom size",
          type: "operation",
          approvers: [{id: 1, name: "Joyce"}],
          width: 120,
          height: 40,
        },
        {
          id: 4,
          x: 240,
          y: 220,
          name: "Operation",
          type: "operation",
          approvers: [{id: 2, name: "Allen"}],
        },
        {
          id: 5,
          x: 440,
          y: 220,
          name: "Operation",
          type: "operation",
          approvers: [{id: 3, name: "Teresa"}],
        },
      ],
      connections: [
        {
          source: {id: 1, position: "right"},
          destination: {id: 4, position: "left"},
          id: 1,
          type: "pass",
        },
        {
          source: {id: 4, position: "right"},
          destination: {id: 5, position: "left"},
          id: 2,
          type: "pass",
        },
        {
          source: {id: 5, position: "right"},
          destination: {id: 2, position: "left"},
          id: 3,
          type: "pass",
        },
        {
          source: {id: 5, position: "bottom"},
          destination: {id: 4, position: "bottom"},
          id: 4,
          type: "reject",
        },
        {
          source: {id: 1, position: "top"},
          destination: {id: 3, position: "left"},
          id: 5,
          type: "pass",
        },
        {
          source: {id: 3, position: "right"},
          destination: {id: 2, position: "top"},
          id: 6,
          type: "pass",
        },
      ],
    };
  },
  methods: {
    handleChartSave(nodes, connections) {
      console.log(nodes);
      console.log(connections);
      // axios.post(url, {nodes, connections}).then(resp => {
      //   this.nodes = resp.data.nodes;
      //   this.connections = resp.data.connections;
      //   // Flowchart will refresh after this.nodes and this.connections changed
      // });
    },
    handleRender(node, children) {
      console.log(node, children);
      children.body.style("fill", 'red');
    },
    handleEditNode(node) {
      if (node.id === 2) {
        console.log(node.description);
      }
    },
    handleEditConnection(connection) {
      console.log(connection);
    },
    handleDblClick(position) {
      this.$refs.chart.add({
        id: +new Date(),
        x: position.x,
        y: position.y,
        name: 'New',
        type: 'operation',
        approvers: [],
      });
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
