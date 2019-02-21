<template>
  <div>
    <div class="days-container">
      <div class="day " v-for="(pre_day,pre_index) in pre_days" :key="`A-${pre_index}`">
      </div>
      <DayCalendar
        v-for="(day,index) in mount" :key="`B-${index}`"
        :day="day"
        :active="isActive(day)"
        :selected_days.sync="local_selected_days"
        :event_key="event_key"
        :start_day.sync="local_start_day"
      />
    </div>
  </div>
</template>

<script>

import DayCalendar from './DayCalendar.vue'

export default {
  name: 'MountCalendar',
  components: {
    DayCalendar
  },
  props: {
    start_day: Number,
    mount: Number,
    selected_days: Array,
    pre_days: Number,
    event_key: Boolean
  },
  data: function () {
    return {
      local_selected_days: [],
      local_start_day: 0
    }
  },
  mounted: function () {
    if (this.selected_days) {
      this.local_selected_days = this.selected_days
    }
    if (this.start_day) {
      this.local_start_day = this.start_day
    }
  },
  watch: {
    local_selected_days: function (val) {
      this.$emit('update:selected_days', val)
    },
    local_start_day: function (val) {
      this.$emit('update:start_day', val)
    }
  },
  methods: {
    isActive (day) {
      if (this.local_selected_days) {
        return this.local_selected_days.indexOf(day) !== -1
      } else {
        return false
      }
    }
  }
}

</script>

<style scoped>
  .days-container {
    margin: auto ;
    display: flex;
    flex-wrap: wrap;
    width: 900px;
  }

  .day {
    background-color: rgb(219, 219, 219);
    border-radius: 4px;
    margin: 12px;
    width: 100px;
    height: 100px;
  }
</style>
