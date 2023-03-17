<script>
  import { tick } from 'svelte'
  import { XMLParser, XMLBuilder } from 'fast-xml-parser'
  const parserOptions = {
    ignoreAttributes: false,
    // attributeNamePrefix: '@_',
    allowBooleanAttributes: true,
    ignorePiTags: false,
    preserveOrder: true,
    parseTagValue: false,
    trimValues: false, //default
    // numberParseOptions: {
    //   leadingZeros: false,
    //   hex: false,
    //   skipLike: /(\+[0-9])|[0-9]\.0+/,
    //   eNotation: false,
    // },
  }
  const xmlParser = new XMLParser(parserOptions)
  const xmlBuilder = new XMLBuilder(parserOptions)
  /* 设置聚焦 */
  function setEndOfContenteditable(contentEditableElement) {
    var range, selection
    if (document.createRange) {
      //Firefox, Chrome, Opera, Safari, IE 9+
      range = document.createRange() //Create a range (a range is a like the selection but invisible)
      range.selectNodeContents(contentEditableElement) //Select the entire contents of the element with the range
      range.collapse(false) //collapse the range to the end point. false means collapse to end rather than the start
      selection = window.getSelection() //get the selection object (allows you to change selection)
      selection.removeAllRanges() //remove any selections already made
      selection.addRange(range) //make the range you have just created the visible selection
    } else if (document.selection) {
      //IE 8 and lower
      range = document.body.createTextRange() //Create a range (a range is a like the selection but invisible)
      range.moveToElementText(contentEditableElement) //Select the entire contents of the element with the range
      range.collapse(false) //collapse the range to the end point. false means collapse to end rather than the start
      range.select() //Select the range (make it the visible selection
    }
  }

  /* 去除尾部br标签 */
  function clearTailBrTag(xml) {
    const xmlJson = xmlParser.parse(xml)
    console.log(xmlJson)
    const jsonXml = xmlBuilder.build(xmlJson)
    console.log(jsonXml)
    return jsonXml
  }

  let rowStartId = 0
  let rowListData = [
    {
      id: 'magicdomid-' + '00' + rowStartId,
      innerHTML: '',
    },
  ]
  function rowInput(event) {
    // console.log(event)
  }
  /* 
  blur处理
  处理nbsp;转换
  */
  /* 
  键盘按键处理
  enter tab 空格 上下左右
  enter键需要换行 处理nbsp;转换
  */
  async function rowKeyDown(event) {
    console.log(event)
    switch (event.key) {
    }
    if (event.key == 'Enter') {
      rowStartId++
      const currentIndex = +event.srcElement.dataset.index
      let innerHtml = event.srcElement.innerHTML
      const nextDomId = 'magicdomid-' + '00' + rowStartId
      rowListData.splice(currentIndex + 1, 0, {
        id: nextDomId,
        innerHTML: '',
      })
      innerHtml = innerHtml.replace(/&nbsp;/g, ' ')
      innerHtml = innerHtml.startsWith('<div class="data-line-wrap">')
        ? innerHtml
        : `<div class="data-line-wrap">${innerHtml}</div>`
      console.log(innerHtml)
      const resetCurrentInnerHtml = clearTailBrTag(innerHtml)
      setTimeout(() => {
        rowListData[currentIndex].innerHTML = resetCurrentInnerHtml
        setEndOfContenteditable(document.querySelector('#' + nextDomId))
      })
    }
  }
  function makeFocus() {
    // const dom = document.querySelector('#base-row')
    // dom.focus()
    // setEndOfContenteditable(dom)
    setEndOfContenteditable(document.querySelector('#magicdomid-001'))
  }
</script>

<div class="row-list-wrap">
  {#each rowListData as row, i}
    <div
      contenteditable="true"
      class="base-row"
      data-index={i}
      id={row.id}
      on:input={rowInput}
      on:keydown={rowKeyDown}
      bind:innerHTML={row.innerHTML}
      spellcheck="false"
    />
  {/each}
</div>

<style lang="scss">
  .base-row {
    // height: 22px;
    background-color: #f5f5f5;
    border: 1px solid #eee;
    font-size: 16px;
    // line-height: 16px;
    padding: 4px;
    outline: none;
    margin: 8px;
    min-height: 16px;
    white-space: pre-wrap;
  }
</style>
