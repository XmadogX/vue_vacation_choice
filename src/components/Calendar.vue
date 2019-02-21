<template>
  <div>
    <b>Год: </b>
    <input
      v-model="year"
      type="number"/>
    <div
      @mousedown="mouseLeft($event,true)"
      @mouseup="mouseLeft($event,false)"
      @mouseleave="mouseLeft($event,false)"
    >
      <div class="calendar" v-for="(mount,index) in mounts" :key="index">
        <b>{{`${name_mounts[mount-1]} (${lenMount(mount) ? `0${mount}`:mount})`}}</b>
        <MountCalendar
          :mount="daysMount(mount-1)"
          :selected_days.sync="selected_days[mount-1]"
          :start_day.sync="start_day"
          :pre_days="preDays(mount)"
          :event_key="event_key"
        />
      </div>
    </div>
  </div>
</template>

<script>

import MountCalendar from './MountCalendar.vue'

export default {
  name: 'Calendar',
  components: {
    MountCalendar
  },
  props: {
    msg: String
  },
  data: function () {
    return {
      start_day: 0,
      event_key: false,
      year: 2019,
      mounts: 12,
      selected_days: [],
      name_mounts: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь']
    }
  },

  mounted: function () {
    for (let index = 0; index < this.mounts; index++) {
      this.selected_days.push([])
    }
  },
  methods: {
    lenMount (mount) {
      return mount.toString().length < 2
    },

    daysMount (mount) {
      return 32 - new Date(this.year, mount, 32).getDate()
    },
    preDays (mount) {
      return new Date(this.year, mount - 1, 0).getDay()
    },
    mouseLeft (event, bool) {
      if (event.button === 0) {
        this.event_key = bool
      }
    }
  }
}
</script>

<style scoped>
  .calendar {
    user-select: none;
    margin: 20px auto auto auto ;
    }
</style>
