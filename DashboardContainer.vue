<template>
  <div class="min-h-screen bg-slate-100">
    <div class="dashboard-container">
      <div class="grid grid-cols-1 gap-6">
        <!-- Subjects section -->
        <section class="w-full">
          <Subjects />
        </section>

        <!-- Procard and Calendar container -->
        <div class="grid grid-cols-1 gap-6">
          <!-- Procard section -->
          <section class="w-full">
            <procard />
          </section>

          <!-- Calendar section -->
          <section class="w-full">
            <Calendar />
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Calendar from "@/components/Educat/calendar/Calendar.vue";
import procard from "@/components/Educat/procard.vue";
import Subjects from "@/components/Educat/Subjects.vue";

export default {
  name: 'DashboardContainer',
  components: {
    Subjects,
    procard,
    Calendar
  }
}
</script>

<style scoped>
.dashboard-container {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  width: calc(100% - 16rem); /* 16rem margin for sidebar */
  margin: 1.5rem; /* Margin around all sides */
  padding: 1.5rem;
  overflow-y: auto;
  background-color: rgb(241 245 249); /* bg-slate-100 */
}

/* For smaller screens */
@media (max-width: 768px) {
  .dashboard-container {
    width: 100%;
    margin: 1rem;
    padding: 1rem;
  }
}
</style>