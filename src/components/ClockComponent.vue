<template>
  <div class="clock-pack">
    
    <img class="ny-time" src="../assets/location.svg" alt="Location-icon">
    
    <div class="clock">
      <p>The City That Never Sleeps</p>
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
  
    <img class="ny-time" src="../assets/clock.svg" alt="Clock-icon">
    
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
  width: 300px;
  padding: 2%;
    border: 3px solid #F4AD28;
    box-shadow: 3px 3px rgb(0 0 0 / 40%);
  margin: auto;
    width: 400px;
    height: 200px;
}
.hour, .minute, .second {
  display: inline-block;
  width: 1.5em;
}

.clock-pack {
  display: flex;
	flex-direction: row;
	flex-wrap: nowrap;
	justify-content: space-around;
	align-items: center;
	align-content: stretch;
  margin-left:30%;
  margin-right: 30%;
}

.ny-time {
  border-radius: 0;
  margin-bottom: 0;
  max-width: 10%;
 max-height: 10%;
}

</style>