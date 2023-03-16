<script>
  import 'carbon-components-svelte/css/white.css'
  import { onMount } from 'svelte'
  import superagent from 'superagent'
  import { goto } from '$app/navigation'

  let folderList = []
  onMount(async function () {
    try {
      const {
        body: { result: res },
      } = await superagent.get('/api/getFolders')
      folderList = res
    } catch (error) {}
  })
  function goIntoFolder(e, folderId) {
    goto(`/folder/1`)
  }
</script>

<h2>文件夹</h2>

<div class="folder-wrap">
  {#each folderList as folder, i}
    <div
      class="folder"
      on:click={e => {
        goIntoFolder(e, folder.id)
      }}
      on:keypress={() => {}}
      role=" button"
    >
      <img class="icon" src="/images/folder.png" alt="文件夹" />
      <div class="name-wrap">
        <span class="name">{folder.name}</span>
        <span class="engName">{folder.engName}</span>
      </div>
    </div>
  {/each}
</div>

<style lang="scss">
  .folder-wrap {
    display: flex;
    flex-wrap: wrap;
    .folder {
      display: flex;
      align-items: center;
      padding: 8px 20px;
      cursor: pointer;
      &:hover,
      &:focus-visible {
        outline: #999 auto 1px;
      }
      .icon {
        width: 60px;
        height: 60px;
      }
      .name-wrap {
        display: flex;
        flex-direction: column;
        padding-left: 10px;
        .name {
          font-size: 14px;
        }
        .engName {
          font-size: 12px;
          color: #999;
        }
      }
    }
  }
</style>
