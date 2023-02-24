<template>
  <div class="clock-pack">
    <div id="clock_ny">
    <div class="clock">
    <span class="hour">{{ hours.toLocaleString('en-US', {
          minimumIntegerDigits: 2,
          useGrouping: false,
        })}}</span>:
    <span class="minute">{{ minutes.toLocaleString('en-US', {
          minimumIntegerDigits: 2,
          useGrouping: false,
        })}}</span>:
    <span class="second">{{ seconds.toLocaleString('en-US', {
          minimumIntegerDigits: 2,
          useGrouping: false,
        })}}</span>
  </div>
</div>
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
  display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
  font-size: 3em;
  font-weight: bold;
  text-align: center;
  background-color: #ECECEC;
  padding: 2%;
  margin: auto;
  width: 30%;
  border-radius: 15px;
  border: 3px solid #F4AD28;    box-shadow: 3px 3px rgba(0, 0, 0, 0.4);
  margin-top: 5%;
  margin-bottom: 5%;
}
@media (max-width: 768px) {
  .clock {
    width: 95%;
    margin-bottom: 20%;
  }
}
.hour, .minute, .second {
  display: inline-block;
  width: 1.5em;
}

</style>