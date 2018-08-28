<template>
  <div class="container">
    <app-logo tabindex="0" />
      <div class="content">
        <timeline class="timeline" @focus="onUpdateFocus" domain="mstdn.jp" stream="public" />
        <div class="content-right">
          <status-detail v-if="focusStatus" v-bind="focusStatus" tabindex="-1" />
            <media-view v-if="focusStatus" v-bind="focusStatus" tabindex="-1" />
        </div>
      </div>
  </div>
</template>

<script>
import AppLogo from '~/components/AppLogo.vue'
import Timeline from '~/components/TL.vue'
import StatusDetail from '~/components/StatusDetail.vue'
import MediaView from '~/components/MediaView.vue'

export default {
  components: {
    AppLogo,
    Timeline,
    StatusDetail,
    MediaView,
  },
  data() {
    return {
      focusStatus: null,
    };
  },
  methods: {
    onUpdateFocus(status) {
      this.focusStatus = status;
    },
  },
}
</script>

<style lang="scss" scoped>
* {
  box-sizing: content-box;
}

.container {
  width: 100%;
  height: 100vh;
  min-height: 100vh;

  display: flex;
  flex-direction: column;
  justify-content: center;

  align-items: center;
  text-align: center;
  font: small/1.2 sans-serif;
}

.content {
  width: 100%;
  flex: 1;

  display: flex;
  flex-direction: row;

  & .timeline {
    border: 1px solid;
    overflow-y: scroll;
  }

  & .content-right {
    width: 600px;
    height: 100%;
    border: 1px solid;
  }
}
</style>
