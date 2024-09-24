<template>
  <div>
    <template v-if="cyInstance">
      <slot :cy="cyInstance"></slot>
    </template>
  </div>
</template>

<script>
import cytoscape from 'cytoscape'
import dagre from 'cytoscape-dagre'
import GraphStyles from './GraphStyles'

export default {
  data() {
    return {
      cyInstance: null,
      addnodes: [
        { id: '9', label: 'add1' },
        { id: '10', label: 'add2' }
      ],
      addedges: [
        { source: '7', target: '9' },
        { source: '9', target: '10' }
      ]
    }
  },
  mounted() {
    cytoscape.use(dagre)

    this.cyInstance = cytoscape({
      container: this.$el,
      style: GraphStyles
    })

    this.cyInstance.on('add', () => {
      this.cyInstance
        .makeLayout({
          name: 'dagre',
          rankDir: 'TB',
          positions: (node) => {
            const existingNode = this.cyInstance.getElementById(node.id())
            // Keep original position for existing nodes
            return existingNode
              ? { x: existingNode.position('x'), y: existingNode.position('y') }
              : undefined
          }
        })
        .run()
    })
  },
  created() {
    this.addElementsWithDelay()
  },
  methods: {
    addElementsWithDelay() {
      const totalElements = Math.max(this.addnodes.length, this.addedges.length)

      for (let i = 0; i < totalElements; i++) {
        setTimeout(() => {
          // Add node if it exists
          if (i < this.addnodes.length) {
            const node = this.cyInstance.add({
              group: 'nodes',
              data: this.addnodes[i]
            })

            // Animate node addition
            node.animate(
              {
                style: {
                  opacity: 0,
                  width: '50px',
                  height: '50px'
                },
                duration: 100
              },
              {
                step: (now) => {
                  node.style('opacity', now)
                  node.style('width', `${50 * now}px`)
                  node.style('height', `${50 * now}px`)
                },
                complete: () => {
                  node.animate({
                    style: {
                      opacity: 1,
                      width: '50px',
                      height: '50px'
                    },
                    duration: 100
                  })
                }
              }
            )
          }

          // Add edge if it exists
          if (i < this.addedges.length) {
            const edge = this.cyInstance.add({
              group: 'edges',
              data: this.addedges[i]
            })

            // Animate edge addition
            edge.animate(
              {
                style: {
                  opacity: 0
                },
                duration: 100
              },
              {
                step: (now) => {
                  edge.style('opacity', now)
                },
                complete: () => {
                  edge.animate({
                    style: {
                      opacity: 1
                    },
                    duration: 100
                  })
                }
              }
            )
          }
        }, i * 1000) // Add every node and edge every 5 seconds
      }
    }
  }
  // methods: {
  //   addElementsWithDelay() {
  //     const totalElements = Math.max(this.addnodes.length, this.addedges.length)

  //     // 先记录所有已有节点的位置
  //     const existingNodePositions = {}
  //     this.cyInstance.nodes().forEach((node) => {
  //       existingNodePositions[node.id()] = node.position()
  //     })

  //     for (let i = 0; i < totalElements; i++) {
  //       setTimeout(() => {
  //         // Add node if it exists
  //         if (i < this.addnodes.length) {
  //           const nodeData = this.addnodes[i]
  //           const node = this.cyInstance.add({
  //             group: 'nodes',
  //             data: nodeData,
  //             position: existingNodePositions[nodeData.id] || {
  //               x: Math.random() * 500,
  //               y: Math.random() * 500
  //             } // 如果没有旧位置则随机
  //           })

  //           // Animate node addition
  //           node.animate(
  //             {
  //               style: {
  //                 opacity: 0,
  //                 width: '50px',
  //                 height: '50px'
  //               },
  //               duration: 100
  //             },
  //             {
  //               step: (now) => {
  //                 node.style('opacity', now)
  //                 node.style('width', `${50 * now}px`)
  //                 node.style('height', `${50 * now}px`)
  //               },
  //               complete: () => {
  //                 node.animate({
  //                   style: {
  //                     opacity: 1,
  //                     width: '50px',
  //                     height: '50px'
  //                   },
  //                   duration: 100
  //                 })
  //               }
  //             }
  //           )
  //         }

  //         // Add edge if it exists
  //         if (i < this.addedges.length) {
  //           const edge = this.cyInstance.add({
  //             group: 'edges',
  //             data: this.addedges[i]
  //           })

  //           // Animate edge addition
  //           edge.animate(
  //             {
  //               style: {
  //                 opacity: 0
  //               },
  //               duration: 100
  //             },
  //             {
  //               step: (now) => {
  //                 edge.style('opacity', now)
  //               },
  //               complete: () => {
  //                 edge.animate({
  //                   style: {
  //                     opacity: 1
  //                   },
  //                   duration: 100
  //                 })
  //               }
  //             }
  //           )
  //         }

  //         this.cyInstance.layout({ name: 'cose', animate: true }).run()
  //       }, i * 1000) // Add every node and edge every second
  //     }
  //   }
  // }
}
</script>
