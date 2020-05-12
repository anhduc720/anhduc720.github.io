<template>
  <Layout :sidebar="true">
    <div class="content">
      <h1 class="logo">{{ $static.metadata.siteName }} - {{ this.description }}</h1>
      <DocListing />
      <GitLink class="git" size="large" />
    </div>
  </Layout>
</template>

<static-query>
query {
  metadata {
    siteName
  }
}
</static-query>

<script>
import GitLink from '~/components/GitLink.vue'
import DocListing from '~/components/DocListing.vue'

export default {
  components: {
    GitLink,
    DocListing
  },
  data() {
    return {
      description: '....'
    }
  },
  metaInfo() {
    return {
      title: this.description,
      meta: [
        { key: 'description', name: 'description', content: 'A theme for static site documentation based on Gridsome, ready to deploy to Netlify in one click.' }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
.content {
  display: flex;
  flex-direction: column;
}

h1 {
  /* text-align: center; */
  /* margin: 1.5em auto 1.5em; */

  /* @include respond-above(md) {
    max-width: 1000px;
  } */
  font-family: Georgia, 'Times New Roman', Times, serif;
}

h2 {
  font-size: 1.4rem;
  margin: 0;
}

nav {
  display: flex;
  justify-content: space-between;
  flex-direction: column;

  /* @include respond-above(sm) {
    flex-direction: row;
  } */
}

.git {
  margin: 3em 0 0;
  align-self: center;

  /* @include respond-above(md) {
    margin: 5em 0 0;
  } */
}
</style>
