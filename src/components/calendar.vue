<template>
  <div class="calendar">
    <div class="handlebar">
      <div class="prev" @click="prevMonth">上个月</div>
      <div class="current">{{ year }} - {{ month }} - {{ day }}</div>
      <div class="next" @click="nextMonth">下个月</div>
    </div>
    <div class="calendar-content">
      <div class="week-list clearfix">
        <div class="week-item">日</div>
        <div class="week-item">一</div>
        <div class="week-item">二</div>
        <div class="week-item">三</div>
        <div class="week-item">四</div>
        <div class="week-item">五</div>
        <div class="week-item">六</div>
      </div>
      <div class="day-content clearfix">
        <div class="day-item prev-month" v-show="prevMonthDay.length > 0" v-for="item in prevMonthDay">{{ item }}</div>
        <div class="day-item" v-for="item in days" :data-date="year+','+month+','+item">{{ item }}</div>
        <div class="day-item next-month" v-show="nextMonthDay.length > 0" v-for="item in nextMonthDay">{{ item }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'calendar',
  data () {
    return {
      year:new Date().getFullYear(),
      month:new Date().getMonth()+1,
      day:new Date().getDate(),
      prevMonthDay:[],
      nextMonthDay:[],
      days:[]
    }
  },
  methods:{
    getDayLengthInMonth(year,month){
      return new Date(year,month,0).getDate();
    },
    getFirstDayWeek(year,month){
      return new Date(year, month - 1, 1).getDay()
    },
    getLastDayWeek(year,month){
      return new Date(year, month, 0).getDay() //0-6
    },
    setCalendar(year,month){
      this.prevMonthDay = [];
      this.nextMonthDay = [];
      this.days = [];

      let prevMonthDayLength = this.getFirstDayWeek(year,month);
      let dayLength = this.getDayLengthInMonth(year,month);
      let nextMonthDayLength = this.getLastDayWeek(year,month);

      for(let i=0;i<prevMonthDayLength;i++){
        this.prevMonthDay.push(i);
      }

      for(let i=1;i<=dayLength;i++){
        this.days.push(i);
      }

      for(let i=1;i<=(6-nextMonthDayLength);i++){
        this.nextMonthDay.push(i);
      }

    },
    prevMonth(){
      this.month -= 1;
      if(this.month<=0){
        this.month = 12;
        this.year -= 1;
      }
      this.setCalendar(this.year,this.month);
    },
    nextMonth(){
      this.month += 1;
      if(this.month>12){
        this.month = 1;
        this.year += 1;
      }
      this.setCalendar(this.year,this.month);
    }
  },
  mounted(){
    this.setCalendar(this.year,this.month);
  }
}
</script>

<style lang="scss" scoped>
  .clearfix:after{
    content:'';
    clear:both;
    display: block;
  }
  .calendar{
    width:560px;
    /*background: #5ccaa4;*/
    .handlebar{
      display: flex;
      justify-content: space-between;
    }
    .calendar-content{
      width: 100%;
      .week-list{
        line-height: 50px;
        height:50px;
        margin-top:10px;
        .week-item{
          float:left;
          width:81px;
          margin-right:-1px;
          box-sizing: border-box;
        }
      }
      .day-content{
        .day-item{
          float:left;
          width:81px;
          border:1px solid #CCC;
          height:50px;
          line-height:50px;
          margin-bottom:-1px;
          margin-right:-1px;
          box-sizing: border-box;
        }
        .prev-month{
          color:#CCC;
          background-color: #EEE;
        }
        .next-month{
          color:#CCC;
          background-color: #EEE;
        }
      }
      
    }
  }
</style>
