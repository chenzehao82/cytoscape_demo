<script>
export default {
  props: ['cy', 'node'],

  mounted() {
    this.cy.add({
      group: 'nodes',
      id: this.node.id,
      data: { ...this.node }
    })
  },

  render() {
    return null
  }
}
</script>
