<template>
  <div class="min-h-screen w-full max-w-4xl mx-auto bg-gray-50 p-5">
    <header class="w-full p-5">
      <h1 class="font-bold text-2xl">Convert Images to SVG</h1>
    </header>

    <main class="grid gap-2 md:grid-cols-2">
      <SectionBox btnBoxClass="justify-between">
        <template v-slot:article>
          <img
            :src="setImage"
            alt="your selected file"
            class="w-full h-full object-contain"
            v-if="setImage !== ''"
          />
          <input
            type="file"
            accept="image/*"
            name="fileInput"
            class="hidden"
            @change="handleChange"
            ref="fileInput"
          />
        </template>
        <template v-slot:buttons>
          <Button :onClick="handleRef" purple="true">Choose Image</Button>

          <Button :onClick="handleUpload" green="true">
            <div v-if="!setUploadStatus">Get SVG</div>
            <div v-else class="flex items-center justify-between space-x-1">
              <Loading />
              <div>Getting SVG</div>
            </div>
          </Button>
        </template>
      </SectionBox>

      <SectionBox btnBoxClass="md:justify-end">
        <template v-slot:article>
          <pre class="h-full w-full px-5 overflow-auto whitespace-pre-wrap">
            <code class="text-xs">{{ `\n${svgDisplay}` }}</code>
          </pre>
        </template>
        <template v-slot:buttons>
          <Button :onClick="copySvg">{{ clipboardStatus }}</Button>
        </template>
      </SectionBox>
    </main>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      setImage: '',
      svgDisplay: '<svg>\n</svg>',
      setUploadStatus: false,
      clipboardStatus: 'Copy to Clipboard',
    }
  },
  methods: {
    handleRef() {},
    handleChange(e) {
      console.log(e)
    },
    async handleUpload() {},
    copySvg() {},
  },
}
</script>
