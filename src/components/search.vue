<script type="text/babel">
  import T from 'libt'

  module.exports = {
    props: {
      model: {
        type: Object,
        default: function () {
          return {
            search: ''
          }
        }
      },
      input: {
        type: Array,
        default: function () {
          return []
        }
      },
      output: {
        type: Array,
        default: function () {
          return []
        }
      },
      onChange: {
        type: Function,
        default () {}
      }
    },
    mounted: function () {
      this.run(this.model.search)
    },
    methods: {
      run: function (newVal, oldVal) {
        var R = []
        var I

        newVal = newVal ? newVal.toLowerCase() : ''
        oldVal = oldVal ? oldVal.toLowerCase() : ''

        if (!oldVal || !newVal || newVal.indexOf(oldVal) === -1) {
          I = this.input
        } else {
          I = this.output
        }

        I.forEach(row => {
          var match = false
          Object.keys(row).forEach(key => {
            if (!match && String(row[key]).toLowerCase().indexOf(newVal) !== -1) {
              match = true
            }
          })
          if (match) {
            R.push(row)
          }
        })

        T.sync(this.output, R)

        this.onChange()
      }
    },
    watch: {
      input: function () {
        this.run(this.model.search)
      },
      'model.search': function (newVal, oldVal) {
        this.run(newVal, oldVal)
      }
    }
  }
</script>

<template>
  <input v-model="model.search" v-bind="$attrs" />
</template>
