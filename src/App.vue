<template>
  <div id="app">
    <Graph class="app-graph">
      <template slot-scope="{ cy }">
        <GraphNode v-for="node in nodes" :key="node.id" :cy="cy" :node="node" />
        <GraphEdge
          v-for="(edge, index) in edges"
          :key="`${edge.source}-${edge.target}-${index}`"
          :cy="cy"
          :edge="edge"
        />
      </template>
    </Graph>
  </div>
</template>

<script>
import Graph from './components/Graph'
import GraphNode from './components/Graph/GraphNode'
import GraphEdge from './components/Graph/GraphEdge'

export default {
  data() {
    return {
      nodes: [
        { id: '1', label: 'Start' },
        { id: '2', label: '4' },
        { id: '3', label: '1' },
        { id: '4', label: '8' },
        { id: '5', label: '15' },
        { id: '6', label: '23' },
        { id: '7', label: '42' },
        { id: '8', label: 'End' }
      ],
      edges: [
        { source: '1', target: '2' },
        { source: '2', target: '3' },
        { source: '3', target: '6' },
        { source: '2', target: '4' },
        { source: '4', target: '5' },
        { source: '5', target: '4', label: '2' },
        { source: '5', target: '6' },
        { source: '6', target: '7' },
        { source: '7', target: '7', label: '3' },
        { source: '7', target: '8' }
      ]
    }
  },
  components: {
    Graph,
    GraphNode,
    GraphEdge
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
}

#app {
  display: flex;
  flex-direction: column;
  width: 100vw;
  height: 100vh;
}

.app-graph {
  flex: 1;
}
</style>
