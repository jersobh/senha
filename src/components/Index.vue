<template>
  <q-layout>
    <div v-if="data.company" slot="header" class="toolbar light">
      <q-toolbar-title :padding="0">
        <img style="max-width:100px;max-height:100px;" :src="data.company.logo"></img>
        {{ data.company.name }}
      </q-toolbar-title>
    </div>

    <div class="layout-view">
      <div class="row width-5of5"">
        <div class="width-4of5">
          <span v-if="data.call.length > 0" class="senha">{{ data.call[0].code }}{{ data.call[0].counter }}</span>
        </div>
        <div class="width-1of5">

        </div>
      </div>
    </div>

  </q-layout>
</template>

<script>

import { Utils, Platform } from 'quasar'
import io from 'socket.io-client'

export default {
  data () {
    return {
      data: {},
      call: [],
      socket: io.connect('http://localhost:3000'),
    }
  },
  computed: {},
  methods: {

  },
  mounted () {
    var self = this
    this.socket.on('loadData', function(data) {
      console.log('recebendo dados')
      self.data = data
    });
    this.socket.on('callNext', function(data) {
      console.log('recebendo dados')
      self.call = data
    });
  },
  beforeDestroy () {
  }
}
</script>

<style lang="stylus">
.senha {
  font-size: 18em;
}
</style>
