<template>
  <div class="profile-page">
    <div class="user-info">
      <div class="container">
        <div class="row">
          <div class="col-xs-12 col-md-10 offset-md-1">
            <img :src="profile.image" class="user-img" />
            <h4>{{ profile.username }}</h4>
            <p>
              {{ profile.bio }}
            </p>
            <button class="btn btn-sm btn-outline-secondary action-btn">
              <i class="ion-plus-round"></i>
              &nbsp; Follow {{ profile.username }}
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="container">
      <div class="row">
        <div class="col-xs-12 col-md-10 offset-md-1">
          <div class="articles-toggle">
            <ul class="nav nav-pills outline-active">
              <li class="nav-item">
                <a class="nav-link active" href="">My Articles</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="">Favorited Articles</a>
              </li>
            </ul>
          </div>

          <div　v-for="article in userArticle.list" :key="article.slug" class="article-preview">
            <div class="article-meta">
              <a href=""><img :src="article.author.image" /></a>
              <div class="info">
                <a href="" class="author">{{article.author.username}}</a>
                <span class="date">{{article.createdAt | date('MMM DD, YYYY')</span>
              </div>
              <button class="btn btn-outline-primary btn-sm pull-xs-right">
                <i class="ion-heart"></i> {{article.favoritesCount}}
              </button>
            </div>
            <a href="" class="preview-link">
              <h1>
                {{article.body}}
              </h1>
              <p>{{article.description}}</p>
              <span>Read more...</span>
              <ul v-if="article.tagList.length > 0" class="tag-list">
                <li v-for="tag in article.tagList" :key="tag" class="tag-default tag-pill tag-outline">{{tag}}</li>
              </ul>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { getUserProfile } from "@/api/profile";
import { getArticles } from "@/api/article";

export default {
  name: "UserProfile",
  middleware: "authenticated",
  async asyncData({ params }) {
    const { data: profileRes } = await getUserProfile(params.username);
    const { data: articlesRes } = await getArticles({
      author: profileRes.profile.username,
    });
    return {
      profile: profileRes.profile,
      userArticle: {
        list: articlesRes.articles,
        count: articlesRes.articlesCount,
      },
    };
  },
  methods: {
    async getUserArticles() {
      const { data } = await getArticles({ author: this.profile.username });
      console.log(data);
    },
  },
};
</script>

<style></style>
