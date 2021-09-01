<template>
    <div class="date-picker">
      <div class="dates">
        <div class="month">
          <div class="arrows prev-mtn" @click="prevMonth">&lt;</div>
          <div class="mth">{{ monthText }} {{ year }}</div>
          <div class="arrows next-mtn" @click="nextMonth">&gt;</div>
        </div>
        <div class="week">
          <div v-for="(item,index) in daysList" :key="index" class="day" >
            {{ item }}
          </div>
        </div>
        <div class="days">
          <div v-for="(item,index) in arrayDays" :key="index" class="day"
               :style="{borderColor: item.current ? '#d9d8d8' : '' } ">
            {{ item.number }}
          </div>
        </div>
      </div>
    </div>
</template>

<script>

export default {
  name: "DatePicker",
  data() {
    return {
      monthList: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'],
      daysList: ['Пн', 'Вт', 'Ср', 'Чт', 'Птн', 'Сб', 'Вс',],
      date: new Date(),
      day: null,
      month: null,
      year: null,
      countDays: null,
      countDaysPrev: null,
      countDaysNext: null,
      startWeekDay: null,
      finishWeekDay: null,
    }
  },
  methods: {
    setDate() {
      this.countDays = new Date(this.year, this.month).daysInMonth()
      this.countDaysPrev = new Date(this.year, this.month === 0 ? 11 : this.month - 1).daysInMonth()
      this.countDaysNext = new Date(this.year, this.month === 11 ? 0 : this.month + 1).daysInMonth()
      this.startWeekDay = new Date(this.year, this.month).getDay() - 1 < 0 ? 1 : new Date(this.year, this.month).getDay() - 1
      this.finishWeekDay = this.startWeekDay + this.countDays
    },
    nextMonth() {
      this.month++
      if (this.month > 11) {
        this.month = 0
        this.year++
      }
      this.setDate()
    },
    mapArrayDate(arr, current){
      return arr.map((ele)=> {return {number:ele, current}})
    },
    prevMonth() {
      this.month--
      if (this.month < 0) {
        this.month = 11
        this.year--
      }
      this.setDate()
    }
  },
  computed: {
    monthText() {
      return this.monthList[this.month]
    },
    arrayDays() {
      return [].concat(
          this.mapArrayDate(this.startWeekDay ? Array.from({length: this.countDaysPrev}, (_, i) => i + 1).splice(-this.startWeekDay) : [],false,),
          this.mapArrayDate(Array.from({length: this.countDays}, (_, i) => i + 1),true),
          this.mapArrayDate(Array.from({length: this.countDaysNext}, (_, i) => i + 1).splice(0, 42 - this.finishWeekDay),false),
      )

    }
  },
  created() {
    this.day = this.date.getDay()
    this.month = this.date.getMonth()
    this.year = this.date.getFullYear()
    Date.prototype.daysInMonth = function () {
      return 33 - new Date(this.getFullYear(), this.getMonth(), 33).getDate()
    }
    this.setDate()


  }

}
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  color: #afacac;
}
.date-picker {
  position: relative;
  width: 100%;
  max-width: 400px;
  background-color: #FFF;
  margin: 30px auto;
  box-shadow: 0px 3px 10px rgba(0, 0, 0, 0.2);
  user-select: none;
}
.date-picker .dates .month {
  display: flex;
  justify-content: space-between;
  align-items: center;

}
.date-picker .dates .week {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  border-bottom: 1px solid #d7d6d6;
  padding: 5px 0;
}
.date-picker .dates .month .arrows {
  cursor: pointer;
  width: 35px;
  height: 35px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
}
.date-picker .dates .month .arrows:hover {
 color: #4b4949;
}
.date-picker .dates .days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);

}
.date-picker .dates .days .day {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  margin: 2px;
  color: #929090;
  border: 1px solid #eeecec;
}



</style>