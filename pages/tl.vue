<template>
  <div class="container">
    <app-logo />
      <div class="content">
        <timeline v-bind:statuses="statuses" />
        <status-detail v-if="statuses[0]" v-bind="statuses[0]" />
      </div>
  </div>
</template>

<script>
import WebSocket from 'isomorphic-ws';

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
      size: 20,
      focus: '',
      statuses: [],
    };
  },
  created() {
    //const domain = 'social.mikutter.hachune.net';
    const domain = 'mstdn.jp';
    const token = '';
    const publicStreamURL = `wss://${domain}/api/v1/streaming/?stream=public&access_token=${token}`;
    const publicStream = new WebSocket(publicStreamURL);
    publicStream.onmessage = (e) => {
      const evt = JSON.parse(e.data);
      switch(evt.event) {
        case 'update':
          {
            const payload = JSON.parse(evt.payload);
            this.statuses.unshift(payload);
            this.statuses.splice(this.size);
            break;
          }
        default:
          console.log(evt.event);
      }
    }
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
