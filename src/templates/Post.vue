<template>
  <Layout>
    <div class="post-title">
      <h1 class="post-title__text">{{ $page.post.title }}</h1>
      <PostMeta :post="$page.post" />
    </div>
    <social-sharing
      :url="`https://www.angularcode.com/${$page.post.path}`"
      :title="$page.post.title"
      :description="$page.post.description"
      :quote="$page.post.description"
      hashtags="serverless,nodejs,vuejs,pm2,nginx"
      twitter-user="itswadesh"
      inline-template
    >
      <div class="fx justify-between items-center cursor-pointer">
        <network network="facebook" style="color:#3B5998" class="w-full">
          <div class="bg-gray-200 py-2 text-center text-xs">
            <i class="fa fa-fw fa-facebook" />Facebook
          </div>
        </network>
        <network network="reddit" style="color:#ff4500" class="w-full">
          <div class="bg-gray-200 py-2 text-center text-xs">
            <i class="fa fa-fw fa-reddit" />Reddit
          </div>
        </network>
        <network network="twitter" style="color:#53A8E7" class="w-full">
          <div class="bg-gray-200 py-2 text-center text-xs">
            <i class="fa fa-fw fa-twitter" />Twitter
          </div>
        </network>
        <network network="whatsapp" style="color:#54CC61" class="w-full">
          <div class="bg-gray-200 py-2 text-center text-xs">
            <i class="fa fa-fw fa-whatsapp" />Whatsapp
          </div>
        </network>
      </div>
    </social-sharing>
    <div class="grid" style="margin:0 auto;max-width: 1195px;">
      <div class="post content-box container">
        <div class="post__header">
          <g-image alt="Cover image" v-if="$page.post.cover_image" :src="$page.post.cover_image" />
        </div>
        <div class="post__content" v-html="$page.post.content" />
        <div class="post__footer">
          <PostTags :post="$page.post" />
        </div>
      </div>
      <!-- <vue-disqus
        shortname="angularcode"
        :identifier="$page.post.title"
      ></vue-disqus>-->
      <div>
        <RightSidebar />
      </div>
    </div>
    <div class="post-comments">
      <!-- Add comment widgets here -->
      <vue-disqus shortname="angularcode" :identifier="$page.post.title"></vue-disqus>
    </div>

    <Author class="post-author" />
  </Layout>
</template>

<script>
import PostMeta from "~/components/PostMeta";
import PostTags from "~/components/PostTags";
import Author from "~/components/Author.vue";
import RightSidebar from "~/components/RightSidebar.vue";
import SocialSharing from "vue-social-sharing";

export default {
  components: {
    Author,
    PostMeta,
    PostTags,
    RightSidebar,
    SocialSharing
  },
  metaInfo() {
    return {
      title: this.$page.post.title,
      meta: [
        { name: "author", content: "Swadesh Behera" },
        {
          key: "description",
          name: "description",
          content: this.$page.post.description
        },
        {
          key: "og:title",
          property: "og:title",
          content: this.$page.post.title
        },
        {
          key: "og:description",
          property: "og:description",
          content: this.$page.post.description
        },
        {
          key: "og:image",
          property: "og:image",
          content: this.$page.post.cover_image
        }
      ]
    };
  }
};
</script>

<page-query>
query Post ($id: ID!) {
  post: post (id: $id) {
    title
    path
    date (format: "D. MMMM YYYY")
    timeToRead
    tags {
      id
      title
      path
    }
    description
    content
    cover_image (width: 860, blur: 10)
  }
}
</page-query>

<style lang="scss">
.justify-between {
  justify-content: space-between;
}
.items-center {
  align-items: center;
}
.cursor-pointer {
  cursor: pointer;
}
.py-2 {
  padding: 1rem 0;
}
.text-center {
  text-align: center;
}
.bg-gray-200:hover {
  background-color: rgba(0, 0, 0, 0.1);
}
.w-full {
  width: 25%;
}
.grid {
  display: flex;
  // grid-gap: 10px;
  // grid-template-columns: repeat(auto-fill, 186px);
}
@media only print {
  .grid {
    display: block;
  }
}

@media only screen and (max-width: 1272px) {
  .grid {
    display: block;
  }
}

.container {
  justify-items: center;
}
.post-title {
  padding: calc(var(--space) / 2) 0 calc(var(--space) / 2);
  text-align: center;
}
.post {
  &__header {
    width: calc(100% + var(--space) * 2);
    margin-left: calc(var(--space) * -1);
    margin-top: calc(var(--space) * -1);
    margin-bottom: calc(var(--space) / 2);
    overflow: hidden;
    border-radius: var(--radius) var(--radius) 0 0;

    img {
      width: 100%;
    }

    &:empty {
      display: none;
    }
  }

  &__content {
    h2:first-child {
      margin-top: 0;
    }

    p:first-of-type {
      font-size: 1.2em;
      color: var(--title-color);
    }

    img {
      width: calc(100% + var(--space) * 2);
      margin-left: calc(var(--space) * -1);
      display: block;
      max-width: none;
    }
  }
}

.post-comments {
  padding: calc(var(--space) / 2);

  &:empty {
    display: none;
  }
}

.post-author {
  margin-top: calc(var(--space) / 2);
}
</style>
