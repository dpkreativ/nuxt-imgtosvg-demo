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
import formatter from 'html-formatter'
import copy from 'copy-to-clipboard'

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
    handleRef() {
      this.$refs.fileInput.click()
    },
    handleChange(e) {
      // console.log(e)
      const reader = new FileReader()

      reader.readAsDataURL(e.target.files[0])

      reader.onload = (onloadEvent) => {
        this.setImage = onloadEvent.target.result
      }
    },
    async handleUpload() {
      this.setUploadStatus = true

      if (this.setImage !== '') {
        // Upload image to Cloudinary
        const uploadImage = await this.$cloudinary
          .upload(this.setImage, {
            upload_preset: 'imgtosvg',
          })
          .then((res) => res.public_id)

        // Convert to SVG
        const url = await this.$cloudinary.image.url(uploadImage, {
          format: 'svg',
          effect: 'vectorize',
        })

        this.setImage = url

        // Extract SVG code
        await this.$axios
          .$get(url)
          .then((data) => (this.svgDisplay = formatter.render(data)))
      } else {
        alert('Choose image first!')
      }

      this.setUploadStatus = false
    },
    copySvg() {
      copy(this.svgDisplay)
      this.clipboardStatus = 'Copied!'
      setTimeout(() => {
        this.clipboardStatus = 'Copy to Clipboard'
      }, 2000)
    },
  },
}
</script>
