<template>
  <b-container class="p-4" fluid>
    <CourseModal :course="selectedCourse" />
    <FullCalendar
      id="schedule"
      default-view="timeGridWeek"
      min-time="06:00:00"
      event-border-color="white"
      height="auto"
      :header="{
        left: '',
        center: '',
        right: '',
      }"
      :all-day-slot="false"
      :weekends="false"
      :column-header-format="{ weekday: 'short' }"
      :plugins="calendarPlugins"
      :events="courseEvents"
      @eventClick="onEventClicked"
    />
  </b-container>
</template>

<script>
import FullCalendar from "@fullcalendar/vue";
import timeGridPlugin from "@fullcalendar/timegrid";
import { mapGetters } from "vuex";
import CourseModal from "@/components/CourseModal";

export default {
  name: "Schedule",
  components: { FullCalendar, CourseModal },
  data() {
    return {
      calendarPlugins: [timeGridPlugin],
      selectedCourse: {},
    };
  },
  computed: {
    ...mapGetters(["currentPlan", "courseEvents"]),
  },
  methods: {
    onEventClicked(info) {
      this.selectedCourse = {
        start: info.event.start,
        end: info.event.end,
        ...info.event.extendedProps.course,
      };
      this.$root.$emit("bv::show::modal", "course-modal");
    },
  },
};
</script>

<style lang="scss">
@import "~@fullcalendar/core/main.css";
@import "~@fullcalendar/timegrid/main.css";

.fc-today {
  background-color: inherit !important;
}

.fc-event {
  cursor: pointer;
}
</style>
