<template>
  <section class="posts">
    sample index
  </section>
</template>

<script>
import { createClient } from '~/plugins/contentful.js'
import siteConfig from '~/siteConfig.json'
import List from '~/components/List.vue'

const client = createClient()

export default {
  head () {
    return {
      title: `${siteConfig.title} | ${siteConfig.subtitle}`,
      meta: [{
        hid: `description`,
        name: 'description',
        content: `${siteConfig.title} | ${siteConfig.subtitle}`,
      }]
    }
  },
  asyncData ({env}) {
    return Promise.all([
      client.getEntries({
        'sys.id': env.CTF_AUTHOR_ID
      }),
      client.getEntries({
        'content_type': 'post'
      }),
      client.getEntries({
        'content_type': 'post',
        'fields.featured': true
      }),
      client.getEntries({
        'content_type': 'post',
        order: '-fields.date'
      }),
      client.getEntries({
        'content_type': 'tag',
        order: '-sys.createdAt'
      })
    ]).then(([authors, posts, postsFeatured, postsLatest, tags]) => {
      return {
        author: authors.items[0],
        posts: posts.items,
        postsFeatured: postsFeatured.items,
        postsLatest: postsLatest.items,
        tags: tags.items
      }
    }).catch(console.error)
  },
  data () {
    return {
      categoriesFeatured: siteConfig.categoriesFeatured
    }
  },
  components: {
    List,
  }
}
</script>

<style lang="stylus">
.posts
  width 98%
  margin 0 auto
</style>

