<template>
  <div>
    <div class="description word-wrap-bz" v-html="the_text"></div>
    <div class="description word-wrap-bz" v-html="description"></div>
    <a v-show="img_url" @click="openImg(img_url)">
      <img :src="img_url" class="ui image padding-top-bz">
    </a>
  </div>
</template>

<script>
  import '../assets/public.css'
  import myautolinker from '../functions/myautolinker'

  export default {
    props: ['message'],
    computed: {
      img_url: function () {
        if (this.message.extended_entities.pictrue) {
          let img_url = this.message.extended_entities.pictrue
          img_url = window.btoa(window.btoa(img_url))
          return '/api_sp/' + img_url
        } else {
          return ''
        }
      },
      description: function () {
        if (this.message.content.description) {
          if (this.message.text) {
            if (this.message.text.includes(this.message.content.description)) return ''
          }
          return myautolinker(this.message.content.description, 'facebook')
        } else {
          return ''
        }
      },
      the_text: function () {
        if (this.message.text) {
          return myautolinker(this.message.text, 'facebook')
        }
        return ''
      }
    },
    methods: {
      openImg: function (img_url) {
        if (this.$route.name === 'TheMessage') { // 在 TheMessage 还点了图，就在新页中打开图
          window.open(img_url, '_blank')
        } else {
          this.$router.push({name: 'TheMessage', params: {id: this.message.id}})
        }
      }
    }
  }
</script>

<style>
</style>
