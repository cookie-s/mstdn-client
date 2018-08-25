<template>
  <div class="container">
    <app-logo />
      <div class="content">
        <timeline @focus="onUpdateFocus" domain="mstdn.jp" stream="public" />
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

<style>
.container {
  width: 100%;
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
</style>
