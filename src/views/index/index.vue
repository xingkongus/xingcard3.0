<template>
  <div class="html">
    <transition name="slide-right">
      <side-bar v-show="showBar"></side-bar>
    </transition>
    <div>
      <title-con class="title-con">
        <transition name="fade">
          <search-box class="title" v-if="isSearch" v-on:goBack="cancelSearch"></search-box>
          <page-title class="title" v-else v-on:searchClick="searchClick" v-on:showBar="showSideBar"></page-title>
        </transition>
      </title-con>
      <contact-con></contact-con>
    </div>
  </div>
</template>

<script>
import titleCon from '@/components/titleCon'
import pageTitle from '@/components/pageTitle'
import searchBox from '@/components/searchBox'
import sideBar from '@/components/sideBar'
import contactCon from '@/components/contactCon'

import { mapMutations, mapGetters, mapState } from 'vuex'
import utils from 'utils'

export default {
  data () {
    return {
      isSearch: false,
      curIdx: 0
    }
  },
  computed: {
    ...mapState({
      showBar: 'isShowBar'
    })
  },
  watch: {
    showBar (value) {
      // 当sidebar弹出时防止滑动右边空白地方让列表滚动
      let body = document.getElementsByTagName('body')[0]
      let stopScroll = (event) => {
        event.preventDefault()
        event.stopPropagation()
        return false
      }
      if (value) {
        utils.addEvent(body, 'touchmove', stopScroll)
      } else {
        utils.removeEvent(body, 'touchmove', stopScroll)
      }
    }
  },
  components: {
    titleCon,
    pageTitle,
    searchBox,
    sideBar,
    contactCon
  },
  methods: {
    select (idx) {
      this.showBar = false
      if (idx === -1) {
        return
      }
      this.title = this.data.departments[idx].name
      this.curIdx = idx
    },
    searchClick () {
      this.isSearch = true
    },
    cancelSearch () {
      this.isSearch = false
    },
    ...mapMutations({
      showSideBar: 'toggleBar'
    })
  }
}
</script>

<style scoped>
.notScroll{
  overflow: hidden;
}

.side-bar{
  transform: translate3d(0, 0, 0);
}

.title-con{
  position: relative;
  width: 100%;
  height: 47px;
  background-color: #ffffff;
}

.title{
  position: absolute;
  width: 100%;
  top: 0;
}

.slide-right-enter-active,
.slide-right-leave-active{
  transition: transform .5s
}

.slide-right-enter,
.slide-right-leave-to{
  transform: translate3d(-100%, 0, 0)
}

.fade-enter-active,
.fade-leave-active{
  transition: opacity .3s
}

.fade-enter,
.fade-leave-to{
  opacity: 0
}

</style>
