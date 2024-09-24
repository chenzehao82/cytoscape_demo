<script>
export default {
  props: ['cy', 'edge'],

  mounted() {
    this.cy.add({
      group: 'edges',
      id: this.edge.id,
      data: { ...this.edge },
      classes: this.edge.label ? 'label' : null
    })
  },

  render() {
    return null
  }
}
</script>
