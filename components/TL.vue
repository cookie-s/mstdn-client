<template>
  <div @keydown.74="down" @keydown.75="up" class="timeline">
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
      latest: null,
      statusesMap: OrderedMap(),
      size: 20,
      focus: '',
    };
  },
  computed: {
    statuses() {
      return [...this.statusesMap.valueSeq().reverse()];
    },
  },
  methods: {
    onUpdateFocus(id) {
      this.focus = id;

      const status = this.statusesMap.get(id);
      if(status) {
        this.$emit('focus', status);
      }
    },
    insertStatus(payload) {
      const id = payload.id;
      payload.prev = '';
      payload.next = this.latest;
      {
        const latest = this.statusesMap.get(this.latest);
        if(latest) {
          latest.prev = id;
        }
      }
      this.latest = payload.id;
      this.statusesMap = this.statusesMap.set(payload.id, payload).takeLast(this.size);
    },
    up() {
      const focus = this.statusesMap.get(this.focus);
      if(focus) {
        const target = focus.prev;
        if(target) {
          this.$el.querySelector(`article.status-${target}`).focus();
        }
      }
    },
    down() {
      const focus = this.statusesMap.get(this.focus);
      if(focus) {
        const target = focus.next;
        if(target) {
          this.$el.querySelector(`article.status-${target}`).focus();
        }
      }
    },
  },
  components: {
    Status,
  },
  async created() {
    const token = '';

    const timelineURL = `https://${this.domain}/api/v1/timelines/${this.stream}`;
    const resp = await axios.get(timelineURL);
    resp.data.forEach(e => this.insertStatus(e));

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
