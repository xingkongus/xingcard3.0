<template>
  <div>
    <div class="col-xs-2"><span class="iconfont icon-back01" style="font-size: 22px" @click="goBack"></span></div>
    <div class="search-box col-xs-8 p-l">
      <div class="row">
        <input ref="searchInput" class="iconfont icon-search form-control search" type="search" v-model="searchQuery" @keyup.enter="doSearch(searchQuery)" placeholder="请输入联系人..." autofocus>
        <span class="clear-btn iconfont icon-cross1 f-s-14" @click="clearSearch"></span>
      </div>
    </div>
    <div class="col-xs-2"><span class="iconfont icon-search" @click="doSearch(searchQuery)"></span></div>
    <div class="clearfix"></div>
  </div>
</template>

<script>
import {mapActions} from 'vuex'
export default {
  mounted () {
    // input始终获得焦点
    this.$refs.searchInput.focus()
  },
  data () {
    return {
      searchQuery: ''
    }
  },
  watch: {
    searchQuery () {
      this.doSearch(this.searchQuery)
    }
  },
  methods: {
    ...mapActions({
      doSearch: 'searchContact'
    }),
    clearSearch () {
      this.searchQuery = ''
    },
    goBack () {
      this.clearSearch()
      this.doSearch('')
      this.$emit('goBack')
    }
  }
}
</script>

<style scoped>
.search-box{
  position: relative;
}
.search-box .search{
  height: 30px;
  padding: 0 10px 0 10px;
  border-width: 1px;
  border-radius: 3px;
  margin-top: 7.5px;
}
.search-box .clear-btn{
  position: absolute;
  top: 0px;
  right: 10px;
  color: #848484;
}
.search-box .search:focus{
  /*border-color: #bdc3 c7;*/
  /*box-shadow: none;*/
  border-color: #5dade2;
  /*outline-color: #5dade2;*/
}
</style>
