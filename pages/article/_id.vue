<template>
  <div class="main">
    <div class="main-container">
      <div class="page-container article-container">
        <div class="article">
          <div class="content" v-html="articleContent"></div>
        </div>
      </div>
      <div class="sidebar">
        <div>
          <Sidebar :articles="hotArticles" :tags="tags"></Sidebar>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Carrousel from '~/components/index/carrousel.vue'
import Sidebar from '~/components/article/sidebar.vue'
import marked from '~/plugins/marked'

export default {
  name: 'article-detail',
  validate ({ params }) {
    return (!!params.id && !Object.is(Number(params.id), NaN))
  },
  fetch ({ store, params }) {
    return store.dispatch('getArticleDetail', params)
  },
  components: {
    Carrousel,
    Sidebar
  },
  data () {
    return {
    }
  },
  computed: {
    article () {
      return this.$store.state.article.detail.data
    },
    articleContent () {
      let content = this.article.content

      if (!content) {
        return ''
      }

      const hasTag = !!this.tags.data.tags.length

      return marked(content, hasTag ? this.tags.data.tags : false, true)
    },
    hotArticles () {
      return this.$store.state.article.hot
    },
    user () {
      return this.$store.state.user
    },
    categories () {
      return this.$store.state.category
    },
    tags () {
      return this.$store.state.tag
    }
  },
  methods: {
  }
}
</script>

<style lang="scss">
.article-container {
  margin-bottom: 2rem;
  padding: 2em 3em;
  background-color: hsla(0, 0%, 100%, .6);
  .article {
    .content {
      a {
        font-weight: bold;
        margin: 0 .1em;

        &.image-link {
          margin: 0;
        }

        &:hover {
          text-decoration: underline;
        }
      }

      img {
        max-width: 100%;
        margin: 0 auto;
        display: block;
        text-align: center;
        border-radius: 2px;
        border: .5rem solid rgba(197, 197, 197, 0.4);
        transition: all .25s;
        opacity: .9;
        cursor: pointer;

        &:hover {
          opacity: 1;
          transition: all .25s;
        }
      }

      p {
        line-height: 2.2rem;
        margin-bottom: 1rem;

        &.text-center {
          text-align: center;
        }

        &.text-right {
          text-align: right;
        }
      }

      h1,
      h2,
      h3,
      h4,
      h5,
      h6 {
        margin: 2rem 0 1rem 0;
        padding-left: 0;
        line-height: 1.8rem;
        font-weight: 700;
        text-indent: 0;
      }

      blockquote {
        p {
          margin-bottom: 0;
        }
      }

      ul {
        list-style-type: square;
      }

      ol {
        list-style-type: decimal;
      }

      ul,
      ol {
        padding-left: 2rem;
        vertical-align: baseline;

        >li {
          line-height: 1.8rem;
          padding: .5rem .8rem;

          &:hover {
            background-color: rgba(230, 230, 230, 0.7);
          }

          >ul {
            &:last-child {
              margin-bottom: 0;
            }
          }

          >p {
            margin-bottom: 0;
          }
        }
      }

      code {
        color: #bd4147;
        padding: .3em .5em;
        margin: 0 .5em;
        border-radius: 2px;
        background-color: rgba(197, 197, 197, 0.4);
      }

      pre {
        display: block;
        position: relative;
        overflow: hidden;
        margin-bottom: 1em;
        padding-left: 2.5em;
        background-color: rgba(0, 0, 0, 0.8);

        &:before {
          color: white;
          content: attr(data-lang)" CODE";
          height: 2.8em;
          line-height: 2.8em;
          font-size: 1em;
          position: absolute;
          top: 0;
          left: 0;
          width: 100%;
          font-weight: 700;
          background-color: rgba(68, 68, 68, 0.9);
          display: block;
          text-transform: uppercase;
          text-align: center;
        }

        >.code-lines {
          position: absolute;
          left: 0;
          top: 2.8em;
          margin: 0;
          padding: 1em 0;
          width: 2.5em;
          height: calc(100% - 2.8em);
          text-align: center;
          background-color: rgba(0, 0, 0, 0.2);

          >.code-line-number {
            padding: 0;
            position: relative;
            list-style-type: none;
            line-height: 1.6em;
            transition: background-color .05s;

            &:hover {
              &:before {
                display: block;
                opacity: 1;
                visibility: visible;
              }
            }

            &:before {
              content: '';
              height: 1.6em;
              position: absolute;
              top: 0;
              left: 2.5em;
              width: 66em;
              background-color: rgba(154, 154, 154, 0.2);
              display: none;
              visibility: hidden;
              opacity: 0;
            }
          }
        }

        >code {
          margin: 0;
          padding: 1em;
          float: left;
          width: 100%;
          height: 100%;
          display: block;
          line-height: 1.6em;
          color: rgba(255, 255, 255, 0.87);
          background-color: transparent;
        }
      }
    }
  }
}
</style>