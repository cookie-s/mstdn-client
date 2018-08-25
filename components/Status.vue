<template>
  <article class="status" v-bind:class="id" @focus="$emit('focus', id)" tabindex="0">
    <img class="avatar" v-bind:src="account.avatar" />
    <div class="status-right">
      <div class="account">
        <span>{{ account.acct }}</span> / <span>{{ account.display_name }}</span>
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

<style scoped lang="scss">
article.status {
  border: 1px solid;
  height: 32px;
  display: flex;
  flex-direction: row;

  text-align: left;
}

img.avatar {
  width: 24px;
  height: 24px;
  margin: auto 5px;
}

.status-right {
  flex: 1;
}

div.content {
  height: 1em;
  /*white-space: nowrap;*/
  overflow: hidden;
  text-overflow: ellipsis;
}


</style>
