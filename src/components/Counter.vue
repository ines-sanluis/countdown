<template>
    <div class="container" v-if="loaded">
      <div class="info">
        <div class="date-text">
          <p>Count down to {{ date }}/{{ month + 1}}/{{ year }} </p>
        </div>
        <div class="expired-text" v-if="expired">Expired!</div>
      </div>
      <div class="countdown">
        <div class="col">
          <div class="time">{{ displayDays }}</div>
          <div class="label">days</div>
        </div>
        <div class="col">
          <div class="time">{{ displayHours }}</div>
          <div class="label">hours</div>
        </div>
        <div class="col">
          <div class="time">{{ displayMinutes }}</div>
          <div class="label">minutes</div>
        </div>
        <div class="col">
          <div class="time">{{ displaySeconds }}</div>
          <div class="label">seconds</div>
        </div>
      </div>
        
    </div>
</template>

<script>

export default {
  props: {
    year: {
      type: Number,
      default: 2021
    },
    month: {
      type: Number,
      default: 1
    },
    date: {
      type: Number,
      default: 1
    },
    hour: {
      type: Number,
      default: 0
    },
    minute: {
      type: Number,
      default: 0
    },
    second: {
      type: Number,
      default: 0
    },
    millisecond: {
      type: Number,
      default: 0
    }
  },
  data: () => ({
    displayDays: 0,
    displayHours: 0,
    displayMinutes: 0,
    displaySeconds: 0,
    loaded: false,
    expired: false
  }),
  computed: {
    _seconds: () => 1000, //milliseconds
    _minutes(){
      return this._seconds * 60;
    },
    _hours(){
      return this._minutes * 60;
    },
    _days(){
      return this._hours * 24;
    },
    end(){
      return new Date(
        this.year,
        this.month,
        this.date,
        this.hour,
        this.minute,
        this.second,
        this.millisecond
      )
    },
    dateAsString(){
      return "'"+
      this.formatNum(this.year)+"'/'"+
      this.formatNum(this.month)+"'/'"+
      this.formatNum(this.day)+"'";
    }
  },
  mounted() {
    this.showRemaining();
  },
  methods: {
    formatNum: num => (num < 10 ? '0' + num : num),
    showRemaining(){ // main method
      const timer = setInterval(() => {
        const now = new Date();
        const distance = this.end.getTime() - now.getTime();

        if(distance < 0){
          clearInterval(timer);
          this.expired = true;
          this.loaded = true;
          return;
        }else{
          this.expired = false;
        }

        const days = Math.floor(distance / this._days);
        const hours = Math.floor((distance % this._days) / this._hours);
        const minutes = Math.floor((distance % this._hours) / this._minutes);
        const seconds = Math.floor((distance % this._minutes) / this._seconds);

        this.displayMinutes = this.formatNum(minutes);
        this.displaySeconds = this.formatNum(seconds);
        this.displayHours = this.formatNum(hours);
        this.displayDays = this.formatNum(days);
        this.loaded = true;
      }, 1000);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
*{
  color: white;
}
.date-text, .expired-text{
  font-size: 30px;
}
.date-text{
  padding-top: 20px;
}
.expired-text{
  padding-bottom: 20px;
}
body{
  background: #e7e7e7;
}
.col{
  background: transparent;
}
.container{
  background: radial-gradient(#f12711, #f5af19);
}
.countdown{
  margin: 0 auto;
  color: white;
  padding: 20px;
  font-size: 20px;
  font-family: Helvetica;
  font-weight: 100;
  column-count: 7;
  column-width: 150px;
  border-radius: 5px;
  height: 160px;
  width: 800px;
  text-align: center;
}
.time{
  font-size: 100px;
}
.label{
  font-size: 30px;
  text-align: center;
}
</style>
