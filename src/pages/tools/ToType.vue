<script setup lang="ts">
const isCopy = ref(false)
function copy() {
  isCopy.value = true
  setTimeout(() => {
    isCopy.value = false
  }, 900)
}
const rawData = ref('')
const convertData = ref()
const commentType = ref<'JsDoc' | 'JsSingle'>('JsDoc')

async function convert() {
  if (rawData.value === '')
    return
  const data = rawData.value.split('\n').map(line => line.trim()).filter(line => line !== '')

  const properties = []
  for (let i = 0; i < data.length; i += 2) {
    const [keyWithDataType, description] = data.slice(i, i + 2)
    const [key, dataType] = keyWithDataType.split('\t')
    const comment = generateComment(description)
    properties.push(createPropertyString(key, dataType, comment))
  }

  convertData.value = `{\n  ${properties.join('\n  ')}\n}`
  // eslint-disable-next-line @typescript-eslint/ban-ts-comment
  // @ts-expect-error
  shiki.getHighlighter({
    theme: 'material-theme-palenight',
    langs: ['js'],
  })
    .then((highlighter: any) => {
      const code = highlighter.codeToHtml(convertData.value, { lang: 'js' })
      const pattern = /<pre[^>]*\sstyle=".*?"[^>]*>/gi
      const result = code.replace(pattern, '<pre class="shiki material-theme-palenight">')

      document.getElementById('output')!.innerHTML = result
    })
}

function generateComment(description: string) {
  if (commentType.value === 'JsSingle')
    return `// ${description}`
  return `/** ${description} */`
}

function createPropertyString(key: string, dataType: string, comment: string) {
  return `${comment}\n  ${key}: ${dataType};`
}
</script>

<template>
  <div>
    <div my-2 btn @click="convert">
      转换
    </div>

    <div mx-auto h-75vh max-w-400 text-left grid="~ cols-1 md:cols-2 gap-10">
      <div>
        <textarea v-model="rawData" border-none outline-none class="block h-100% w-100% border-rd bg-#f7f7f7 px-5 py-2 dark:bg-#242424" />
      </div>
      <div class="overflow-hidden border-rd bg-#292d3e px-4 pt-2 text-white dark:bg-#242424">
        <div flex="~ justify-between items-center" h-10 text-sm op30>
          <button
            flex="~ justify-between items-center gap-2" rd-2 px-2 py-2 hover:bg-coolgray
            @click="copy"
          >
            <div i-carbon-copy />
            <span v-show="isCopy">Copied</span>
          </button>
          <span>Typescript</span>
        </div>
        <div id="output" />
      </div>
    </div>
  </div>
</template>
