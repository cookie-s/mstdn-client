<template>
  <div class="timeline">
    <div class="statuses" v-for="status in statuses" :key="status.id">
      <status v-bind="status" @focus="onUpdateFocus" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import {OrderedMap} from 'immutable';
import WebSocket from 'isomorphic-ws';

import Status from '~/components/Status.vue'


export default {
  props: ['domain', 'stream'],
  data() {
    return {
      size: 20,
      statusesMap: OrderedMap(),
    };
  },
  computed: {
    statuses() {
      return [...this.statusesMap.valueSeq().reverse()];
    },
  },
  methods: {
    onUpdateFocus(id) {
      const status = this.statusesMap.get(id);
      if(status) {
        this.$emit('focus', status);
      }
    },
    insertStatus(payload) {
      this.statusesMap = this.statusesMap.set(payload.id, payload).takeLast(this.size);
    },
  },
  components: {
    Status,
  },
  async created() {
    const token = '';

    const timelineURL = `https://${this.domain}/api/v1/timelines/${this.stream}`;
    const resp = await axios.get(timelineURL);
    this.statusesMap = OrderedMap(resp.data.map(e => [e.id, e]));

    const streamURL = `wss://${this.domain}/api/v1/streaming/?stream=${this.stream}&access_token=${token}`;
    const stream = new WebSocket(streamURL);
    stream.onmessage = (e) => {
      const evt = JSON.parse(e.data);
      switch(evt.event) {
        case 'update':
          {
            const payload = JSON.parse(evt.payload);
            this.insertStatus(payload);
            break;
          }
        default:
          console.log(evt.event);
      }
    }
  },
}
</script>

<style scoped>
.timeline {
  display: block;
  flex: 1;
}
</style>
