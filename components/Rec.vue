<template>
  <section class="rec">
    <div class="section-title">
      <h2>{{ labelRec }}</h2>
    </div>
    <LineItem v-for="post in limitBy(posts,showNumRec)"
              :key="post.sys.id"
              :post="post" />      
  </section>
</template>

<script>
import LineItem from '~/components/LineItem.vue'
import Vue2Filters from 'vue2-filters'
import siteConfig from '~/siteConfig.json'

export default {
  data () {
    return {
      showNumRec: siteConfig.listOption.showNumRec,
      labelRec: siteConfig.listOption.labelRec
    }
  },
  props: ['posts'],
  components: {
    LineItem
  },
  mixins: [Vue2Filters.mixin]
}
</script>

<style lang="stylus" scoped>
.rec
  padding 0 5px
</style>


