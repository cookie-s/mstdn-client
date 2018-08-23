<template>
  <article class="status" v-bind:class="id" tabindex="0">
    <img class="avatar" v-bind:src="account.avatar" />
    <div>
      <div class="account">
        <span><a v-bind:href="account.url">{{ account.acct }}</a></span> / <span>{{ account.display_name }}</span>
      </div>
        <div class="content">{{ contentText }}</div>
      </div>
  </article>
</template>

<script>
import textversion from 'textversionjs';

export default {
  props: ['id', 'account', 'content'],
  computed: {
    contentText() {
      return textversion(this.content, {
        linkProcess(href, text) { return text; },
        imgProcess(src, alt) { return '[img]'; },
      });
    },
  },
  data() {
    return {};
  },
}
</script>

<style scoped>
article.status {
  border: 1px solid;
  width: 800px;
  height: 32px;
  display: flex;

  text-align: left;
}

img.avatar {
  width: 24px;
  height: 24px;
  margin: auto 5px;
}

div.content {
  width: 750px;

  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

div.content >>> * {
  display: inline;
  overflow: hidden;
  text-overflow: ellipsis;
}

div.content >>> br {
  display: none;
}
</style>
