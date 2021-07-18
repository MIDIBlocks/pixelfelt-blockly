<template lang="pug">
q-page
  .min-height-inherit.q-pt-appbar
    CodeEditor.min-height-inherit(ref='code' :cursor='{lineNumber: currentEditor.cursor.lineNumber || 6, column: currentEditor.cursor.column || 9}' @onCodeChange='onCodeChange' :value='code' :extraOptions='{language: "javascript"}')
</template>

<script>
import CodeEditor from '../components/CodeEditor'
import {set} from 'lodash'

/**
 * @todo document
 */
export default {
  name: 'FactoryHome',

  components: {CodeEditor},

  data () {
    return {
      currentEditor: {
        code: '',
        cursor: {
          column: 9,
          lineNumber: 6
        }
      },
      
      code: ''
    }
  },

  mounted () {
    set(window, 'app.$page', this)

    this.$mousetrap.bindGlobal('ctrl+`', ev => {
      ev.preventDefault()
      this.runCode()
    })

    this.$root.$on('editor.runCode', this.runCode)
    this.$root.$on('editor.loadCode', this.loadCode)
    this.$root.$on('showDebugger', this.showDebugger)
    window.addEventListener('message', this.onMessage)
  },

  destroyed () {
    this.$mousetrap.unbind('ctrl+`')
    this.$root.$off('editor.runCode', this.runCode)
    this.$root.$off('editor.loadCode', this.loadCode)
    this.$root.$off('showDebugger', this.showDebugger)
    window.removeEventListener('message', this.onMessage)
  }, 

  methods: {
    autosave () {
      window.parent?.postMessage({
        action: 'sitemodder.editor.autosave',
        code: this.currentEditor.code
      }, '*')
    },
    
    /**
     * Handles code editor changes
     */
    onCodeChange (code) {
      this.currentEditor.code = code
      this.autosave()
    },

    /**
     * Runs the code in the parent frame (or current frame if there is no parent)
     */
    runCode () {
      window.parent?.postMessage({
        action: 'sitemodder.editor.runCode',
        code: this.currentEditor.code
      }, '*')
    },

    /**
     * Tell parent window to show debugger in Picture in Picture
     */
    showDebugger () {
      window.parent?.postMessage({action: 'sitemodder.showDebugger'}, '*')
    },

    /**
     * Load code from parent frame
     */
    loadCode (event) {
      this.$refs.code.code = event.data.code
    }
  }
}
</script>
