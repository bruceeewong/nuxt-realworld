<template>
  <div class="article-page">
    <div class="banner">
      <div class="container">
        <h1>{{ article.title }}</h1>

        <article-meta :article="article" />
      </div>
    </div>

    <div class="container page">
      <div class="row article-content">
        <div class="col-md-12" v-html="article.body"></div>
      </div>

      <hr />

      <div class="article-actions">
        <article-meta :article="article" />
      </div>

      <article-comments :article="article" />
    </div>
  </div>
</template>

<script>
import { getArticle } from "@/api/article";
import MarkdownIt from "markdown-it";
import ArticleMeta from "./components/article-meta";
import ArticleComments from "./components/article-comments";

export default {
  name: "PostArticle",
  components: {
    ArticleMeta,
    ArticleComments,
  },
  async asyncData({ params }) {
    const { data } = await getArticle(params.slug);
    const { article } = data;

    const md = new MarkdownIt();

    return {
      article: {
        ...article,
        body: md.render(article.body), // markdown 解析
      },
    };
  },
};
</script>

<style></style>
