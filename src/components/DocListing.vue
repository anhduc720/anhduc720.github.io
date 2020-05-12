<template>
  <div class="doc-listing">
    <article v-for="{ node } in $static.docs.edges" :key="node.slug">
      <g-link :to="node.path" class="doc-title">
        <h3> {{node.title}} </h3>
      </g-link>
      <div class="meta-area">
        <abbr class="mr-3">
          <calendar-icon size="0.5x" class="svg-icon"></calendar-icon> {{last_updated(node.date)}}
        </abbr>
        <Tag v-for="tag in node.tags" :title="tag.title" :link="tag.path" :key="tag.title" />
      </div>
      <p> {{node.description}} </p>
      <g-link :to="node.path" class="readmore">readmore...</g-link>
    </article>
  </div>
</template>


<static-query>
query allDoc {
  docs: allDoc(order: DESC) {
    edges {
      node {
        title
        path
        date
        path
        tags {
          title
          path
        }
        timeToRead
      }
    }

  }
}
</static-query>

<script>
import { CalendarIcon } from 'vue-feather-icons'

import Tag from '~/components/Tag.vue'


export default {
  components: {
    CalendarIcon,
    Tag
  },
  methods: {
    last_updated: string_date => {
      let options = {year: 'numeric', month: 'long', day: 'numeric' };
      let date =  new Date(string_date);
      return date.toLocaleDateString(options);
    }
  }
}
</script>

<style lang="scss" scoped>
  .doc-listing {
    article {
      flex: 1 0;
      border: 1px solid shade($sidebarBright, 10%);
      background: $sidebarBright;
      padding: 2rem;
      border-radius: 3px;
      transition: background .15s ease-in-out, border-color .15s ease-in-out;
      margin-bottom: 1rem;

      .doc-title {
        text-decoration-line: none;
      }

      .dark & {
        border: 1px solid shade($sidebarDark, 10%);
        background: $sidebarDark;
      }
    }
}

</style>