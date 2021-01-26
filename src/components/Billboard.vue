<template>
  <div class="billboard">
    <div class="inner">
      <div
        ref="swiper"
        class="swiper-container">
        <div class="swiper-wrapper">
          <div
            v-for="item in billboards"
            class="swiper-slide">
            <img
              :data-src="item.src"
              class="swiper-lazy" />
            <div class="swiper-lazy-preloader">
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Swiper from "swiper/bundle";
import 'swiper/swiper-bundle.css'

export default {
  data () {
    return {
      billboards: {}
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    async init () {
      this.billboards = await this.$fetch({
        requestName: 'billboards'
      })

      this.$nextTick(() => {
        new Swiper(this.$refs.swiper, {
          effect: 'fade',
          speed: 1000,
          autoplay: {
            delay: 3000
          },
          loop: true,
          preloadImages: false,
          lazy: {
            loadPrevNext: true,
            loadPrevNextAmount: 2
          }
          on: {
            slideChange: () => {
              
            }
          }
        })
      })
    }
  }
}
</script>

<style scoped lang="scss">
  .billboard {
    .inner {
      .swiper-container {
        width: 1240px;
        height: 400px;
      }
    }
  }
</style>