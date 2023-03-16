<script>
  import 'carbon-components-svelte/css/white.css'
  import { onMount } from 'svelte'
  import superagent from 'superagent'
  import { goto } from '$app/navigation'
  import { DataTable } from 'carbon-components-svelte'
  import { Button } from 'carbon-components-svelte'

  let tableDataList = []
  onMount(async function () {
    try {
      const {
        body: { result: res },
      } = await superagent.post('/api/getFiles')
      tableDataList = res
    } catch (error) {}
  })
  function edit(e, row, cell) {
    goto(`/docs/1`)
  }
  function publish(e, row, cell) {}
</script>

<div class="files-table-wrap">
  <DataTable
    stickyHeader
    headers={[
      { key: 'name', value: '名称' },
      { key: 'engName', value: '英文名称' },
      { key: 'creator', value: '创建人' },
      { key: 'creatTime', value: '创建时间' },
      { key: 'updateTime', value: '更新时间' },
      { key: 'hasPublished', value: '首次发布' }, //已发布 未发布
      { key: 'status', value: '当前状态' }, //已发布 未发布
      { key: 'handle', value: '操作' },
    ]}
    rows={tableDataList}
  >
    <strong slot="title">《联盟》</strong>
    <span slot="description"> 文件夹 </span>
    <svelte:fragment slot="cell" let:row let:cell>
      {#if cell.key === 'handle'}
        <Button
          size="small"
          kind="danger-ghost"
          on:click={e => {
            edit(e, row, cell)
          }}>编辑</Button
        >
        <Button
          size="small"
          kind="danger-ghost"
          on:click={e => {
            publish(e, row, cell)
          }}>发布</Button
        >
      {:else}
        {cell.value}
      {/if}
    </svelte:fragment>
  </DataTable>
</div>

<svelte:head>
  <style lang="scss">
    .files-table-wrap {
      table {
        tbody {
          tr {
            td {
              padding-top: 0 !important;
              padding-bottom: 0 !important;
              line-height: 46px;
            }
          }
        }
      }
    }
  </style>
</svelte:head>
