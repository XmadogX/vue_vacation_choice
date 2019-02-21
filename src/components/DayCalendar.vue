<template>
  <div
    @mousedown="clickSelectDay()"
    @mouseover="overSelectDay()"
  >
    <div
      class="day active"
      v-if="active"
    >
      <b>{{day}}</b>
    </div>
    <div
      class="day disable"
      v-else
    >
      <b>{{day}}</b>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DayCalendar',
  props: {
    start_day: Number,
    day: Number,
    selected_days: Array,
    active: Boolean,
    event_key: Boolean
  },
  data: function () {
    return {
      local_selected_days: []
    }
  },
  methods: {
    addSelect (day) {
      this.local_selected_days.push(day)
      this.$emit('update:selected_days', this.local_selected_days)
      this.$emit('update:start_day', day)
    },
    dellSelect (day) {
      this.local_selected_days = this.local_selected_days.filter(item => item !== day)
      this.$emit('update:selected_days', this.local_selected_days)
      this.$emit('update:start_day', day)
    },
    selectedDay (day) {
      if (this.selected_days.indexOf(day) === -1) {
        this.addSelect(day)
      } else {
        this.dellSelect(day)
      }
    },
    overSelectDay () {
      this.local_selected_days = this.selected_days
      if (this.event_key) {
        if (this.start_day !== 0) {
          if (this.start_day <= this.day) {
            for (let day = this.start_day + 1; day <= this.day; day++) {
              this.selectedDay(day)
            }
          } else {
            for (let day = this.start_day - 1; day >= this.day; day--) {
              this.selectedDay(day)
            }
          }
        } else {
          this.selectedDay(this.day)
        }
      }
    },
    clickSelectDay () {
      this.selectedDay(this.day)
    }
  }
}
</script>

<style scoped>
  .day{
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 10px;
      width: 100px;
      height: 100px;
      border-radius: 4px;
    }
  .active {
    border: 2px solid #c50000;
    color:#ff0404;
    background-color: #ffecec;
  }
  .disable {
    border: 2px solid black;
    color:black;
    background-color: #ffffff;
  }
</style>
