<template>
  <div von-app>
    <navbar></navbar>
    <transition name="view">
      <router-view class="view"></router-view>
    </transition>
  </div>

  <modal ref="modal"></modal>
</template>
<style lang='scss'>
  @import "../scss/vonic";
</style>
<script>
  import Vue from 'vue'
  import Navbar from './Navbar'
  import { Modal } from '../modal'

  import utils from './utils'
  import channel from './channel'

  Vue.directive('nav', (data) => {
    channel.$emit('PageTransitionEvent', data)
  })

  // mini services
  import './services/loading'
  import './services/dialog'
  import './services/cascadepanel'

  export default {
    components: {
      Navbar,
      Modal
    },

    data() {
      return {
        gradeClass: 'grade-a'
      }
    },

    mounted() {
      // grade-a for ios, grade-b for android & other
      if (utils.is_ios_device()) {
        this.gradeClass = 'grade-a'
      } else {
        this.gradeClass = 'grade-b'
      }

      this.$nextTick(function(){
        document.querySelector('body').className = this.gradeClass
      })

      // Model Service
      window.$vonicModal = this.$refs.modal

      channel.$on('VonicNotification', (data) => {
        $toast.show(data.message);
      })
    }
  }
</script>
