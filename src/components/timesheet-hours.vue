<template>
  <div class="timesheet-hours">
    <div class="timesheet">
      <div class="timesheet__top-annotate top-annotate">
        <div class="top-annotate__cell" v-for="(day, idx) in daysRange" :key="idx">
          {{ day.date }} {{ day.title }}
        </div>
      </div>
      <div class="timesheet__side-annotate">
        <div class="cell side-annotate" v-for="(hour, idx) in hours" :key="idx">
          <div class="side-annotate__hour">{{ hour }}</div>
          <div class="side-annotate__half">{{ hour }}:30</div>
        </div>
      </div>
      <div class="timesheet__week week">
        <div class="week__day day" v-for="(_, idx) in daysRange" :key="idx">
          <div class="day__timer" :style="{top: `${percent}%`}" v-if="idx === 0"></div>
          <div class="cell cell__booked"></div>
          <div class="cell hour" v-for="(hour, idx) in hours" :key="idx">
            <div class="hour__30"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component({})
export default class TimersheetHours extends Vue {
  /**
   * TODO: prop
   */
  private range = 70;

  /**
   * TODO: prop
   */
  private startDay: Date = new Date();

  /**
   * TODO: prop
   */
  private hours = [8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22];

  private time: Date = new Date();

  private timer: number = 0;

  get percent(): number {
    const firstActiveHour = this.hours[0];
    const lastActiveHour = this.hours[this.hours.length - 1] + 1;

    const currentHour = this.time.getHours();
    const currentMinute = this.time.getMinutes();

    const totalMinutes = lastActiveHour * 60 - firstActiveHour * 60;
    const currentMinutes =
      currentHour * 60 + currentMinute - firstActiveHour * 60;

    const percent = (currentMinutes / totalMinutes) * 100;

    return percent < 100 ? percent : 100;
  }

  get daysRange(): Array<{ title: string; date: number }> {
    const startDate = this.startDay.getDate();
    const weekDay = this.startDay.getDay();
    const weekTitles = ["Вс.", "Пн.", "Вт.", "Ср.", "Чт.", "Пт.", "Сб."];

    return [...new Array(this.range)].map((el, idx) => ({
      title: weekTitles[(weekDay + idx) % weekTitles.length],
      date: new Date(
        new Date(this.startDay.getTime()).setDate(startDate + idx)
      ).getDate()
    }));
  }

  mounted() {
    this.timer = window.setInterval(() => {
      this.time = new Date();
    }, 1000 * 60);
  }

  destroyed() {
    clearInterval(this.timer);
  }
}
</script>

<style lang="scss" scoped>
$timesheet-hours-side-annotate-width: 45px;
$timesheet-hours-top-annotate-height: 30px;
$timesheet-hours-cell-height: 40px;
$timesheet-hours-cell-width: 200px;
$timesheet-hours-cell-border-color: #ddd;

.timesheet-hours {
  background-color: #fff;
}

.timesheet {
  overflow: auto;
  display: flex;
  position: relative;
  padding-top: $timesheet-hours-top-annotate-height;

  &__top-annotate {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: $timesheet-hours-top-annotate-height;
    margin-left: $timesheet-hours-side-annotate-width;
  }

  &__side-annotate {
    position: sticky;
    top: 0;
    left: 0;
    z-index: 1;
    width: $timesheet-hours-side-annotate-width;
  }

  &__week {
    flex-grow: 1;
  }
}

.cell {
  display: flex;
  justify-content: center;
  align-items: center;
  height: $timesheet-hours-cell-height;
  border-bottom: 1px solid $timesheet-hours-cell-border-color;
  border-right: 1px solid $timesheet-hours-cell-border-color;
  font-family: Roboto, sans-serif;
  font-weight: 300;
  text-align: center;
  font-size: 13px;
  background-color: #fff;

  &__booked {
    position: absolute;
    width: 100%;
    background-color: #e8e8e8;
    border-left: 3px solid darken(#c8c8c8, 3);
    height: 200px;
    width: 96%;
    margin-left: 2%;
  }
}

.side-annotate {
  display: flex;
  flex-direction: column;
  justify-content: space-around;

  &__hour {
    align-self: flex-end;
    margin-right: 5px;
    font-size: 11px;
  }

  &__half {
    font-size: 9px;
    align-self: flex-end;
    margin-right: 5px;
    color: #555;
  }
}

.top-annotate {
  display: flex;
  padding: 3px;

  &__cell {
    display: flex;
    justify-content: center;
    align-items: flex-end;
    flex: 0 0 $timesheet-hours-cell-width;
    text-align: center;
    font-family: Roboto, sans-serif;
    font-size: 11px;
    color: #555;
  }
}

.week {
  display: flex;

  &__day {
    flex: 0 0 $timesheet-hours-cell-width;
  }
}

.day {
  position: relative;
  overflow: hidden;

  &__timer {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 1px;
    background-color: red;
  }
}

.hour {
  &__30 {
    width: 100%;
    border-bottom: 1px dashed lighten($timesheet-hours-cell-border-color, 6);
  }
}
</style>
