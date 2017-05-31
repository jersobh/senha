<template>
  <q-layout>
    <div v-if="data.company" slot="header" class="toolbar light">
      <q-toolbar-title :padding="0">
        <img style="max-width:100px;max-height:100px;" :src="data.company.logo"></img>
        {{ data.company.name }}
      </q-toolbar-title>
      <q-select
  type="radio"
  v-model="selectedSector"
  :options="data.sector"
></q-select>
      <button class="primary" @click="next()">
  Próximo
</button>
    </div>

    <div class="layout-view">
      <div class="row width-5of5"">
        <div class="width-4of5">
          <span v-if="call.length > 0" class="senha">{{ call[0].sector.code }}{{ call[0].sector.counter }}</span>
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
      data: {
        company: {
          name: '',
          logo: ''
        },
        sector: [
          {
            label: '',
            value: '',
            counter: 0
          }
        ]
      },
      call: [],
      selectedSector: null,
      socket: io.connect('http://localhost:3000',{secure: true}),
    }
  },
  computed: {},
  methods: {
    next () {
      var self = this
      this.socket.emit('next', self.selectedSector);
    }
  },
  mounted () {
    var self = this
    this.socket.emit('connected');
    this.socket.on('loadData', function(data) {
      self.data = data
    });
  },
  beforeDestroy () {
  }
}
</script>

<style lang="stylus">
.logo-container
  width 192px
  height 268px
  perspective 800px
  position absolute
  top 50%
  left 50%
  transform translateX(-50%) translateY(-50%)
.logo
  position absolute
  transform-style preserve-3d
</style>
