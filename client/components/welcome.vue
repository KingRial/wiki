<template lang='pug'>
  v-app
    .onboarding
      .onboarding-content
        img.animated.fadeIn(:src='config.logoUrl', :alt='config.title')
        .headline.animated.fadeInUp {{ config.title }}
        .subtitle-1.mt-3.animated.fadeInUp.wait-p1s {{ $t('welcome.subtitle') }}
        v-btn.mt-5.animated.fadeInUp.wait-p2s(color='primary', :href='`/e/` + locale + `/home`', x-large)
          v-icon(left) mdi-plus
          span {{ $t('welcome.createhome') }}
</template>

<script>
import _ from 'lodash'
import gql from 'graphql-tag'

export default {
  props: {
    locale: {
      type: String,
      default: 'en'
    }
  },
  data() {
    return {
      config: {
        title: '',
        logoUrl: ''
      }
    }
  },
  apollo: {
    config: {
      query: gql`
        {
          site {
            config {
              title
              logoUrl
            }
          }
        }
      `,
      fetchPolicy: 'network-only',
      update: (data) => _.cloneDeep(data.site.config),
      watchLoading (isLoading) {
        this.$store.commit(`loading${isLoading ? 'Start' : 'Stop'}`, 'admin-site-refresh')
      }
    }
  }
}
</script>

<style lang='scss'>

</style>
