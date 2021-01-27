<template>
  <div>
    <header :class="{ fixed: isFixed }">
      <div class="inner">
        <div
          class="open-nav-drawer"
          @click="onNav">
        </div>
        <a
          href="javascript:void(0)"
          class="logo"></a>
        <div class="search">
          <input
            v-model="searchText"
            placeholder="찾고 싶은 상품을 검색해 보세요!"
            type="text"
            @keyup.enter="search" />
          <!-- v-on:keyup == @keyup-->
          <div
            class="search__icon"
            @click="search"></div>
        </div>
        <div class="ranking">
          <!-- Slider main container -->
          <div
            ref="swiper"
            class="swiper-container">
            <!-- Additional required wrapper -->
            <div class="swiper-wrapper">
              <div
                v-for="(rank, index) in rankings.rankings"
                :key="rank.name"
                class="swiper-slide">
                <a :href="rank.href">
                  <span class="index">{{ index + 1 }}</span>
                  <span class="name">{{ rank.name }}</span>
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </header>
  </div>
</template>
<script>
import _throttle from 'lodash/throttle'
import Swiper from 'swiper';
import 'swiper/swiper-bundle.css';

export default {
  data() {
    return {
      searchText : '',
      rankings: {},
      isFixed: false,
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    async init() {
      window.addEventListener('scroll', _throttle(event => {
        this.isFixed = window.scrollY > 120
      }, 100))

      this.rankings = await this.$fetch({
        requestName: 'rankings'
      })

      this.$nextTick(() => {
        new Swiper(this.$refs.swiper, {
          direction: 'vertical',
          speed: 1000,
          autoplay: {
            delay: 3000
          },
          loop: true
        })
      })
    },
    onNav() {
      //dispatch는 actions를 실행해주는 함수.
      // navigation 내부의 onNav라는 action을 실행하겠다.
      this.$store.dispatch('navigation/onNav')
      console.log('myheaderclick')
    },
    async search() {
      if(!this.searchText.trim()) return;
      const res = await this.$search({
        searchText: this.searchText
      });
      console.log(res);
    }
  }
}
</script>
<style scoped lang="scss">
header{
  background-color: #fff;
  &.fixed {
    width: 100%;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 97;
    box-shadow: 0 2px 8px rgba(#000, .07);
    .inner {
      height: 80px;
    }
  }
  .inner{
    display: flex;
    align-items: center;
    height: 120px;
    .open-nav-drawer{
      width: 60px;
      height: 60px;
      border-radius: 50%;
      cursor: pointer;
      box-shadow:
          0 2px 6px rgba(#000, .06),
          0 0 1px rgba(#000, .4);
      display:flex;
      justify-content: center;
      align-items: center;
      &::after {
        content: "";
        display: block;
        width: 36px;
        height: 36px;
        background-image: url("https://trusting-williams-8cacfb.netlify.app/images/globals_2x.png");
        background-position: -302px -203px;
        background-size: 363px;
      }
    }
    .logo {
      display: block;
      width: 94px;
      height: 40px;
      margin: 0 24px;
      background-image: url("https://trusting-williams-8cacfb.netlify.app/images/globals_2x.png");
      background-position: -162px 0;
      background-size: 363px;
      cursor: pointer;
    }
    .search {
      position: relative;
      input {
        //input의 default display값은 inline-block
        width: 500px;
        height: 50px;
        padding: 0 27px;
        border: 1px solid #ddd;
        border-radius: 25px;
        box-sizing: border-box;
        background-color: #FAFAFA;
        font-size: 18px;
        outline: none;
        font-family: 'Noto Sans KR', sans-serif;
        &::placeholder {
        }
      }
      &__icon{
        width: 50px;
        height: 50px;
        position: absolute;
        top: 0;
        right: 0;
        cursor: pointer;
        background-image: url("https://trusting-williams-8cacfb.netlify.app/images/globals_2x.png");
        background-position: -162px -45px;
        background-size: 363px;
      }
    }
    .ranking {
      width: 210px;
      margin: 0 30px;
      .swiper-container {
        width: 182px;
        height: 28px;
        .swiper-slide {
          a {
            display: block;
            height: 28px;
            line-height: 28px;
            text-decoration: none;
            font-size: 15px;
            color: #333;
            font-weight: 700;
            span.index {
              margin-right: 10px;
              color: #f43142;
              font-style: italic;
            }
            &:hover span.name {
              color: #f43142;
            }
          }
        }
      }
    }
  }
}
</style>