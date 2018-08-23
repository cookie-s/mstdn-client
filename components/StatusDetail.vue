<template>
  <article class="status-detail">
    <div class="account">
      <img class="avatar" v-bind:src="account.avatar" />
      <div class="name">
        <div class="acct"><a v-bind:href="account.url">{{ account.acct }}</a></div>
        <div class="display_name">{{ account.display_name }}</div>
      </div>
    </div>
    <div>
      <div class="content" v-html="content" />
      </div>
  </article>
</template>

<script>
export default {
  props: ['id', 'account', 'content'],
  data() {
    return {};
  },
};
</script>

<style scoped>
article.status-detail {
  border: 1px solid;
  width: 300px;
  align-items: left;
  text-align: left;
}

img.avatar {
  width: 64px;
  height: 64px;
}

div.account {
  display: flex;
  flex-direction: row;
}
div.name {
  display: block;
}
</style>
