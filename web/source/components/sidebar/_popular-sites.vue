<template lang="jade">
h2 人気サイト
ul
  li.sidebar-list(v-repeat="site: sites", v-component="_site")
a.more(href="javascript:void(0)" v-on="click: showMore" v-if="doesHaveMore") もっと見る
</template>

<script lang="coffee">
Site = require '../../site'
module.exports =
  data: ->
    sites: []
  components:
    _site: require './_site.vue'
  computed:
    doesHaveMore: ->
      @sites.length % 10 == 0
  methods:
    showMore: ->
      Site.popular(
        offset: @sites.length
        (err, sites)=>
          @sites = @sites.concat(sites)
      )
  created: ->
    Site.popular(null, (err, @sites)=>)
    @$watch 'sites', -> @$dispatch 'sidebarChanged'
</script>
