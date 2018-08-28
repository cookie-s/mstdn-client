<template>
  <article class="media-view">
    <img class="media-attachment"
         v-for="attachment in media_attachments"
         :key="attachment.id"
         :src="attachment.url" />
  </article>
</template>

<script>
export default {
  props: ['media_attachments'],
  data() {
    return {};
  },
  beforeUpdate() {
    this.$el.querySelector('div.status-content a');
  },
};
</script>

<style scoped lang="scss">
article.media-view {
  height: 50%;
  border: 1px solid;

  & img {
    max-width: 100%;
    max-height: 100%;
  }
}

</style>
