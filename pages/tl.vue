<template>
  <section class="container">
    <div>
      <app-logo />
        <div class="statuses" v-for="status in statuses" :key="status.id">
          <status
                              v-bind="status"
                              />
        </div>
    </div>
  </section>
</template>

<script>
import AppLogo from '~/components/AppLogo.vue'
import Status from '~/components/Status.vue'
const WebSocket = require('isomorphic-ws');

export default {
  components: {
    AppLogo,
    Status,
  },
  data() {
    return {
      size: 20,
      statuses: [],
    };
  },
  created() {
    const domain = 'social.mikutter.hachune.net';
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
            //this.statuses.splice(this.size);
            console.log(payload);
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
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>

