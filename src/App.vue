<template lang="pug">
#q-app(:key='reloads')
  router-view
  #stats-wrap

  //- Error: Generic
  //- @todo I don't think this is being used
  q-dialog(v-model='!!errors.generic' persistent)
    q-card.bg-negative
      q-card-section.row.items-center
        q-avatar.text-negative(icon='fas fa-exclamation-triangle' color='white')
        span.q-ml-sm.text-white {{errors.generic}}
      q-card-actions(align='right')
        q-btn.text-black(@click='goHome' color='white') Go to home page

  Prompt
</template>

<script>
import {set} from 'lodash'
import Prompt from './components/Prompt'
import {mapState} from 'vuex'
import defaultWorkspace from './assets/workspaces/default'
import store from 'store'
import Stats from 'stats.js'

export default {
  name: 'App',

  components: {
    Prompt
  },

  computed: {
    ...mapState(['reloads', 'settings'])
  },

  data () {
    const stats = new Stats()

    return {
      // The handsfree.js loader
      loaderDismiss: null,
      
      // Will display different modals based on error messages
      errors: {
        generic: ''
      },

      stats
    }
  },

  watch: {
    /**
     * Toggles FPS/Stats
     */
    settings: {
      deep: true,
      handler (settings) {
        if (settings.isStatsVisible) {
          document.querySelector('#stats-wrap').classList.remove('hidden')
        } else {
          document.querySelector('#stats-wrap').classList.add('hidden')
        }
      }
    }
  },

  mounted () {
    this.$root.$on('prepareRoute', this.prepareRoute)
    this.$root.$on('error', this.onError)
    set(window, 'app.version', this.$v)
    set(window, 'app.$', this)

    // Check version
    if (!store.get('version') || store.get('version') < this.$v) {
      if (store.get('version')) {
        this.$q.notify({
          message: '📦 Midiblocks has been updated!',
          type: 'positive',
          timeout: 5000
        })
      }
      localStorage.clear()
    }

    // Load default workspace if no localstorage
    if (!localStorage.blocks) {
      Object.keys(defaultWorkspace.localStorage).forEach(key => {
        store.set(key, defaultWorkspace.localStorage[key])
      })
      this.$store.commit('set', ['blocks', defaultWorkspace.localStorage.blocks])
      this.$store.commit('set', ['midiblocks', defaultWorkspace.localStorage.midiblocks])
    }

    // Use new version
    store.set('version', this.$v)

    /**
     * Override notify to update status
     * @todo Move this to a utility module
     */
    const origNotify = this.$q.notify
    this.$q.notify = (...args) => {
      args.forEach(arg => {
        this.$store.commit('set', ['lastEvent', {log: arg.message}])
      })
      return origNotify(...args)
    }

    /**
     * Override console warnings and errors
     */
    const err = console.error
    console.error = (...args) => {
      this.$store.commit('push', ['eventLogs.error', {log: args[0]}])
      err(...args)
    }
    const warn = console.warn
    console.warn = (...args) => {
      this.$store.commit('push', ['eventLogs.warn', {log: args[0]}])
      warn(...args)
    }

    /**
     * Close loader
     */
    document.addEventListener('handsfree-modelLoaded', this.closeLoader)
    document.addEventListener('handsfree-modelError', this.onLoaderError)

    /**
     * @todo Remove after beta
     */
    this.$q.notify({
      position: 'center',
      timeout: 2500,
      message: 'This is still a prototype and may be buggy 😅',
      color: 'ansi-bright-green'
    })

    // Handle messages (from parent frame)
    window.addEventListener('message', (event) => {
      switch (event.data.action) {
        case 'editor.loadCode':
          this.$root.$emit('editor.loadCode', event)
        break
        case 'editor.runCode':
          this.$root.$emit('editor.runCode')
        break
      }
    })

    // Final setup
    document.querySelector('#stats-wrap').appendChild(this.stats.dom)
    this.$store.commit('set', ['settings.isStatsVisible', store.get('settings.isStatsVisible', false)])

    // Let parent frame know that everything is loaded
    this.$nextTick(() => {
      window.parent?.postMessage({action: 'sitemodder.ready'}, '*')
    })
  },

  destroyed () {
    this.$root.$off('error', this.onError)
    document.removeEventListener('handsfree-modelLoaded', this.closeLoader)
    document.removeEventListener('handsfree-modelError', this.onLoaderError)
  },

  methods: {
    onError (payload) {
      this.errors.generic = payload
    },

    goHome () {
      // this.$router.push({name: 'Home'})
      this.errors.generic = ''
    },

    /**
     * Show error when model loading fails
     * @todo Make this a dialog and have a button to report
     */
    onLoaderError () {
      this.$q.notify({
        position: center,
        timeout: 5000,
        message: 'There was an error in loading the face tracker, please refresh the page and try again.',
        color: 'negative'
      })
      this.closeLoader()
    },

    closeLoader () {
      this.loaderDismiss && this.loaderDismiss()
      this.loaderDismiss = null
    }
  }
}
</script>
