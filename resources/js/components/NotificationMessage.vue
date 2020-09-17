<template>
  <div>
    <div class="notification table table-fixed w-full"
         :class="{ 'cursor-pointer': this.notification.data.url }"
         @click="handleClick">
        <span class="table-cell w-8 align-top py-4">
          <span v-if="notification.data.level === 'success'">
            <img src="https://img.icons8.com/flat_round/32/000000/chat.png"/>
          </span>
          <span v-if="notification.data.level === 'info'">
           <img src="https://img.icons8.com/doodle/32/000000/checklist--v1.png"/>
          </span>
          <span v-if="notification.data.level === 'warning'">
            <img src="https://img.icons8.com/color/32/000000/mamits.png"/>
          </span>
          <span v-if="notification.data.level === 'error'">
            <img src="https://img.icons8.com/flat_round/32/000000/chat.png"/>
          </span>
        </span>
      <div class="table-cell w-full py-4 pl-4">
        <!-- <p>{{ notification.data.message }}</p> -->
            <p v-html="notification.data.message"></p>
        <span class="text-sm text-70">{{ notification.created_at | fromNow }}</span>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'NotificationMessage',
    props: [
      'notification'
    ],
    filters: {
      fromNow (date) {
        return new moment.tz(date.date, 'YYYY-MM-DD HH:mm:ss', date.timezone).local().fromNow()
      }
    },
    methods: {
      handleClick() {
        if (this.notification.data.url) {
          let win = window.open("https://" + location.hostname + this.notification.data.url,
                  this.notification.data.target || '_blank')

          if (win) {
            win.focus()
          }
        }
      }
    },
    created () {
      // Set interval to force update every minute
      this.interval = setInterval(() => this.$forceUpdate(), 60000)
    },
    beforeDestroy () {
      clearInterval(this.interval)
    }
  }
</script>
