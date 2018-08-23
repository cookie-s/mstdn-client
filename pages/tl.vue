<template>
  <section class="container">
    <div>
      <app-logo />
        <timeline v-bind:statuses="statuses" />
    </div>
  </section>
</template>

<script>
import AppLogo from '~/components/AppLogo.vue'
import Timeline from '~/components/TL.vue'
const WebSocket = require('isomorphic-ws');

export default {
  components: {
    AppLogo,
    Timeline,
  },
  data() {
    return {
      size: 20,
      focus: '',
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
