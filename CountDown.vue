<template lang="html">
  <!-- CountDown -->
  <div :class="componentClass + secondsClass">
    {{ seconds | timer }}
  </div>
</template>

<script>
  import moment from 'moment'
  import 'moment-duration-format'

  import Vue from 'vue'

  // Counter start event
  const COUNTER_START    = 'counter-start'
  const COUNTER_END      = 'counter-end'
  const DEADLINE_REACHED = 'deadline-reached'

  // Create time view filter
  Vue.filter('timer', function(seconds) {
    var m_seconds = moment.duration(seconds, 'seconds')
    return m_seconds.format(seconds <= 60 ? 'ss[s]'  : null)
  })


  export default {
    data () {
      return {
        seconds: 0,
        secondsClass: '',
      }
    },
    props: [
      'start',
      'countdown',
      'componentClass'
    ],
    computed: {
      // True when counter starts
      counterStart: function () {
        return false;
      },
      // Starting date
      createdDate: function () {
        return moment(this.start * 1000)
      },
      // Countdown timer value
      countDown: function () {
        return Number(this.countdown) * 60
      },
      // Get the difference between now and created date.
      dateDiff: function () {
        return moment().diff(this.createdDate, 'seconds')
      },
      // Calculate deadline
      deadlineDate: function () {
        var deadline = this.countDown - this.dateDiff;
        // If valid deadline return
        if (deadline > 0) {
          return deadline
        } else {
          // Emit DEADLINE_REACHED event
          this.$emit(DEADLINE_REACHED)
        }
      }
    },
    watch: {
      // Every time seconds change check if it's 0 and then emit DEADLINE_REACHED event
      seconds: function (newValue) {
        if (newValue <= 60) {
          // Add is-seconds class
          this.secondsClass = ' is-seconds'
        }
        if (newValue == 0) {
          this.$emit(DEADLINE_REACHED)
        }
      }
    },
    mounted () {
      var vm = this
      // Set seconds equal to deadlineDate
      this.seconds = this.deadlineDate

      // Create interval function to call each second
      var timer = setInterval(function () {
        if (vm.seconds > 0) {
          if (!this.counterStart) {
            vm.$emit(COUNTER_START)
            this.counterStart = true
          }
          vm.seconds--
        } else {
          vm.$emit(COUNTER_END)
          return clearInterval(timer)
        }
      }, 1000)
    }
  }
</script>

<style lang="css">
</style>

