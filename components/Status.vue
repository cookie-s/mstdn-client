<template>
  <article class="status">
    <img class="avatar" v-bind:src="account.avatar" />
    <div>
      <div class="account">
        <span><a v-bind:href="account.url">{{ account.acct }}</a></span> / <span>{{ account.display_name }}</span>
      </div>
      <div class="content" v-html="content" />
      </div>
  </article>
</template>

<script>
export default {
  props: ['account', 'content'],
  data() {
    return {};
  },
}
</script>

<style scoped>
article.status {
  border: 1px solid;
  width: 1000px;
  height: 50px;
  display: flex;

  text-align: left;
}

img.avatar {
  width: 48px;
  height: 48px;
  margin: auto 5px;
}

div.content {
  width: 930px;

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
