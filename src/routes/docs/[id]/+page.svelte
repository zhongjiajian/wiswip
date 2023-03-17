<script>
  import Header from '../header.svelte'
  import BaseRow from '$routes/editor/baseRow.svelte'
</script>

<Header />
<div class="editor-panel">
  <BaseRow />
</div>

<style lang="scss">
  .editor-panel {
    padding: 10px;
  }
</style>
