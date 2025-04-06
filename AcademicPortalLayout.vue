<template>

    <!-- Sidebar -->

    <!-- Main Content -->

      <!-- Subjects Background -->



      <DashboardContainer />

</template>

<script>
import Calendar from "@/components/Educat/calendar/Calendar.vue";
import procard from "@/components/Educat/procard.vue";
import Subjects from "@/components/Educat/Subjects.vue";
import AcademicPortalNavigation from "@/components/Educat/AcademicPortalNavigation.vue";
import DashboardContainer from "@/components/Educat/DashboardContainer.vue";

export default {
  name: 'AcademicPortalLayout',
  components: {
    DashboardContainer,
    AcademicPortalNavigation,
    Subjects,
    procard,
    Calendar
  }
}
</script>

<style scoped>
.portal-container {
  min-height: 100vh;
}

.sidebar {
  position: sticky;
  top: 0;
  height: 100vh;
}

.main {
  position: relative;
}

.subjects-background {
  position: absolute;
  inset: 0;
  z-index: 0;
  opacity: 0.1; /* Set opacity to make it look like a background */
}
</style>
