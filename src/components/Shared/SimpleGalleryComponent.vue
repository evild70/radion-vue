<template>
  <div class="SimpleGalleryComponent">
    <img class="SimpleGalleryComponent_CurrentImage" :src="currentImage | cloudinary('screenshot_big')">
    <div class="SimpleGalleryComponent_Thumbs">
      <img v-for="(image, index) in images"
           class="SimpleGalleryComponent_Thumbs_Item"
           :class="{ current : isCurrent(index) }"
           :src="image | cloudinary('cover_small')"
           @click="setCurrent(index)">
    </div>
  </div>
</template>

<script>
  export default {
    name: 'simple-gallery-component',
    props: {
      images: {
        type: Array,
        required: true,
        validator: function (images) {
          for (let val of images) {
            if (typeof val !== 'string') {
              return false
            }
          }
          return true
        }
      }
    },
    data () {
      return {
        current: 0
      }
    },
    computed: {
      imagesCount () {
        return this.images.length
      },
      currentImage () {
        return this.images[this.current]
      }
    },
    methods: {
      setCurrent (index) {
        this.current = index
      },
      isCurrent (index) {
        return index === this.current
      }
    }
  }
</script>

<style scoped lang="scss" >
  .SimpleGalleryComponent {
    display: flex;
    flex-flow: row wrap;

    &_CurrentImage {
      width: 100%;
      height: 100%;
    }

    &_Thumbs {
      margin-top: 5px;
      white-space: nowrap;
      overflow-x: auto;

      &_Item {
        display: inline-block;
        object-fit: contain;
        width: 160px;
        height: 90px;
        margin-right: 5px;
        transition: transform .2s ease;

        &.current {
          transform: scale(.9);
        }
      }
    }
  }
</style>
