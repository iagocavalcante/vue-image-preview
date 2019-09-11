Iago Cavalcante, [11.09.19 17:52]
<template>
  <div>
    <div class='crop-container'>
      <div :id="cropId"></div>
    </div>

    <div class="preview-buttons">
      <div class="start">
        <input
          type="file"
          :id="inputFileId"
          name="input-file"
          accept="image/*"
          @change="previewThumbnail($event)"
          hidden
        />
        <label class="btn-upload" :for="inputFileId" role="button">
          Selecionar arquivo
        </label>
      </div>
      <div class="end">
        <button type="button"
          class="btn-upload zoom zoom-out"
          @click.prevent="zoomOut()">
          <i class="fas fa-search-minus"></i>
          {{textMinus}}
        </button>
        <button type="button"
        class="btn-upload zoom"
          @click.prevent="zoomIn()">
          <i class="fas fa-search-plus"></i>
          {{textPlus}}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Cropper from 'js-cropper'

export default {
  name: 'OImagePreview',
  props: {
    width: {
      type: Number,
      default: 500,
      required: false
    },
    height: {
      type: Number,
      default: 245,
      required: false
    },
    textMinus: {
      type: String,
      default: '',
      required: false
    },
    textPlus: {
      type: String,
      default: '',
      required: false
    }
  },
  data: () => ({
    img: 'https://images.pexels.com/photos/226746/pexels-photo-226746.jpeg',
    cropId: `crop${Math.random().toString(36).replace(/[^a-z]+/g, '').substr(0, 5)}`,
    inputFileId: `INPUT${Math.random().toString(36).replace(/[^a-z]+/g, '').substr(0, 5)}`,
    cropper: {},
    initialThumbnail: '',
    zoomCount: 0
  }),
  mounted () {
    this.cropper = new Cropper({
      height: this.height
    })
    this.cropper.render(`#${this.cropId}`)
    this.disableDefaultTools()
  },
  methods: {
    zoomIn () {
      this.zoomCount += 0.1
      if (this.zoomCount > 1) {
        this.zoomCount = 1
      }
      this.cropper.setZoom(this.zoomCount)
    },
    zoomOut () {
      this.zoomCount -= 0.1
      if (this.zoomCount < 0) {
        this.zoomCount = 0
      }
      this.cropper.setZoom(this.zoomCount)
    },
    disableDefaultTools () {
      const tools = document.querySelector('.cropper-tools')
      tools.style.display = 'none'
    },
    previewThumbnail (e) {
      let reader = new FileReader()
      const file = e.target.files[0]

      if (!file.type.includes('image/')) return

      reader.onload = (event) => {
        this.cropper.loadImage(event.target.result).then(function (crop) {
          console.info('Image is ready to crop.')
        })
        // this.cropper.replace(event.target.result)
        this.isDefaultPreview = false
      }

      reader.readAsDataURL(file)
    }
  }
}
</script>

<style scoped>
.cropper {
  height: 245px;
  background: #f4f4f4;
}
audio, canvas, progress, video {
  width: 10%
}
.cropper canvas {
  width: 10%
}


.preview-buttons {
  display: flex;
  flex: 1;
  flex-direction: row;
  justify-content: space-between;
  width: 100%;
}

.start {
  align-items: flex-start;
}

.end {
  align-items: flex-end;
}

.zoom-out {
  margin-right: 8px;
}

.btn-upload {
  cursor: pointer;
  height: 36px;
  min-width: 42px;
  padding: 0 16px;
  font-size: 14px;
  font-weight: bold;
  font-style: normal;
  font-stretch: normal;
  line-height: 1.71;
  letter-spacing: normal;
  text-align: center;
  color: #009be1;
  border-radius: 3px;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  border-radius: 4px;
  display: -webkit-inline-box;
  display: -ms-inline-flexbox;
  display: inline-flex;
  -webkit-box-flex: 0;
  -ms-flex: 0 0 auto;
  flex: 0 0 auto;
  letter-spacing: 0.0892857143em;
  -webkit-box-pack: center;
  -ms-flex-pack: center;
  justify-content: center;
  max-width: 100%;
  outline: 0;
  position: relative;
  text-decoration: none;
  text-indent: 0.0892857143em;
  -webkit-transition-duration: 0.28s;
  transition-duration: 0.28s;
  -webkit-transition-property: opacity, -webkit-box-shadow, -webkit-transform;
  transition-property: opacity, -webkit-box-shadow, -webkit-transform;
  transition-property: box-shadow, transform, opacity;
  transition-property: box-shadow, transform, opacity, -webkit-box-shadow, -webkit-transform;
  -webkit-transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  vertical-align: middle;
  white-space: nowrap;
  box-shadow: none !important;
  background-color: transparent;
  border: thin solid currentColor;
}

.btn-upload.zoom {
  padding: 0px;
}
</style>
