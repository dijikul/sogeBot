<template>
<div class="card widget">
  <div class="card-header">
    <ul class="nav nav-pills" role="tablist">
      <li role="presentation" class="nav-item">
        <a class="nav-link active" href="#commercial-main" aria-controls="home" role="tab" data-toggle="tab" title="Commercials">
          <font-awesome-icon icon="dollar-sign" />
        </a>
      </li>
      <li role="presentation" class="nav-item">
        <strong class="text-info nav-link" style="margin-top: 5px;" v-if="countdown > 0">
          <font-awesome-icon :icon="['far', 'clock']" />
          {{ countdown }}s
        </strong>
      </li>
      <li class="nav-item ml-auto">
        <h6 class="widget-title">{{ commons.translate('widget-title-commercial') }}</h6>
      </li>
    </ul>
  </div>

  <!-- Tab panes -->
  <div class="card-body">
    <div class="tab-content">
      <div role="tabpanel" class="tab-pane active" id="commercial-main">
        <div class="text-center row row-widget">
          <div class="col col-widget" v-for="second of seconds" :key="second">
            <button type="button" class="btn btn-outline-secondary btn-widget" v-on:click="run(second)">{{second}}s</button>
          </div>
        </div>
      </div>
      <!-- /MAIN -->
    </div>
  </div>
</div>
</template>

<script>
import { library } from '@fortawesome/fontawesome-svg-core'
import { faDollarSign } from '@fortawesome/free-solid-svg-icons'
import { faClock } from '@fortawesome/free-regular-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(faDollarSign, faClock)

export default {
  props: ['commons', 'socket'],
  components: {
    'font-awesome-icon': FontAwesomeIcon
  },
  data: function () {
    return {
      seconds: [30, 60, 90, 120, 150, 180],
      countdown: 0
    }
  },
  mounted: function () {
    this.$emit('mounted')
  },
  created: function () {
    setInterval(() => {
      if (this.countdown > 0) {
        this.countdown--
      }
    }, 1000)
  },
  methods: {
    run: function (seconds) {
      this.countdown = seconds
      this.socket.emit('commercial.run', { seconds })
    }
  }
}
</script>
