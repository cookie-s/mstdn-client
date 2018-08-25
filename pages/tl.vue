<template>
  <div class="container">
    <app-logo />
      <div class="content">
        <timeline class="timeline" @focus="onUpdateFocus" domain="mstdn.jp" stream="public" />
        <status-detail v-if="focusStatus" v-bind="focusStatus" />
      </div>
  </div>
</template>

<script>
import AppLogo from '~/components/AppLogo.vue'
import Timeline from '~/components/TL.vue'
import StatusDetail from '~/components/StatusDetail.vue'

export default {
  components: {
    AppLogo,
    Timeline,
    StatusDetail,
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

<style scoped>
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
}

.timeline {
  border: 1px solid;
  overflow-y: scroll;
}
</style>
