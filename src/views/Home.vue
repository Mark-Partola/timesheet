<template>
  <div class="page-layout">
    <div class="page-layout__sidebar">
      <layout-sidebar />
    </div>
    <div class="page-layout__workarea">
      <timesheet-hours />
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import LayoutSidebar from "@/components/layout-sidebar.vue";
import TimesheetHours from "@/components/timesheet-hours.vue";

@Component({
  components: {
    LayoutSidebar,
    TimesheetHours
  }
})
export default class Home extends Vue {}
</script>

<style lang="scss" scoped>
.page-layout {
  height: 100vh;
  display: flex;
  background-color: #eee;

  &__sidebar {
    flex: 0 0 100px;
  }

  &__workarea {
    flex-grow: 1;
    overflow-x: hidden;
    margin: 10px 0 0 10px;
  }
}
</style>
