<template>
  <article class="status-detail">
    <div class="account">
      <img class="avatar" :src="account.avatar" />
      <div class="name">
        <div class="acct"><a target="_blank" :href="account.url">{{ account.acct }}</a></div>
        <div class="display_name">{{ account.display_name }}</div>
      </div>
    </div>
    <div class="status-content" v-html="content" />
  </article>
</template>

<script>
export default {
  props: ['id', 'account', 'content'],
  data() {
    return {};
  },
  beforeUpdate() {
    this.$el.querySelector('div.status-content a');
  },
};
</script>

<style scoped lang="scss">
article.status-detail {
  width: 600px;
  height: 250px;
  border: 1px solid;

  display: flex;
  flex-direction: column;

  text-align: left;

  & .account {
    display: flex;
    flex-direction: row;

    & img.avatar {
      width: 64px;
      height: 64px;
    }

    & div.name {
      display: block;
    }
  }

  div.status-content {
    flex: 1;
    overflow: scroll;
  }
}

</style>
