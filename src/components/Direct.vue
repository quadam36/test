<template>
  <div class="direct">
    <div class="inner">
      <ul>
        <li
          v-for="item in directs"
          :key="item.name">
          <a :href="item.href">
            <div>
              <img
                :src="item.src"
                :alt="item.name"
                width="90" />
              <div class="text">
                {{ item.name }}
              </div>
            </div>
          </a>
        </li>
        <li>
          <div
            class="open-more"
            @click="onNav('LNB')">
            <div class="icon"></div>
            <div class="text">
              더보기
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      directs: []
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    async init() {
      this.directs = await this.$fetch({
        // 큰따옴표와 작은따옴표 중 한 가지 방식만 사용하는 것이 좋아요~
        requestName: "directs"
      })
    },
    onNav() {
      this.$store.dispatch('navigation/onNav')
    }
  }
}
</script>

<style scoped lang="scss">
.direct {
  .inner {
    padding: 50px 0;
  }
  ul {
    display: flex;
    li {
      margin-right: 25px;
      &:last-child {
        margin-right: 0;
      }
      img {
        display: block;
        margin-bottom: 15px;
      }
      .text {
        font-size: 15px;
        text-align: center;
      }
      .open-more {
        cursor: pointer;
        .icon {
          width: 90px;
          height: 90px;
          margin-bottom: 15px;
          background-image: url("https://trusting-williams-8cacfb.netlify.app/images/main_2x.png");
          background-position: 0 0;
          background-size: 209px;
        }
      }
    }
  }
}
</style>
