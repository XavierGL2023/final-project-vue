<template>
    <div class="clock">
    <span class="hour">{{ hours }}</span>:
    <span class="minute">{{ minutes }}</span>:
    <span class="second">{{ seconds }}</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      hours: '',
      minutes: '',
      seconds: ''
    }
  },
  mounted() {
    setInterval(() => {
      const now = new Date()
      this.hours = now.getHours()
      this.minutes = now.getMinutes()
      this.seconds = now.getSeconds()
    }, 1000)
  }
}

</script>

<style scoped>
.clock {
  display: inline-block;
  font-size: 3em;
  font-weight: bold;
  text-align: center;
  padding: 0.5em;
  background-color: #eee;
  padding: 2%;
  border-radius: 1em;
  width: 300px;
}
.hour, .minute, .second {
  display: inline-block;
  width: 1.5em;
}

</style>