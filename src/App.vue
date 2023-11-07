<template>
  <main>
    <Navbar />
    <SidebarData />
    <div class="container">
      <div class="row">
        <div class="col-md-3 bg-light sidebar">
          <SidebarVisibility
            @showBackdrop="this.showBackdrop= $event"
            @showActivities="this.showActivities= $event"
            @showActivityLabels="this.showActivityLabels= $event"
            @showActivityShadows="this.showActivityShadows= $event"
            @showEdges="this.showEdges= $event"
          />
        </div>
        <div class="col-md-9 ps-3">
          <h3 class="py-3 mb-3 border-bottom">Visualization</h3>
          <Tiramisu
              :show-backdrop="showBackdrop"
              :show-activities="showActivities"
              :show-activity-labels="showActivityLabels"
              :show-activity-shadows="showActivityShadows"
              :show-edges="showEdges"
          />
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import Tiramisu from "./components/Tiramisu.vue";
import SidebarData from "./components/SidebarData.vue";
import SidebarVisibility from "./components/SidebarVisibility.vue";

export default {
  name: "App",
  components: {
    Navbar,
    Tiramisu,
    SidebarData,
    SidebarVisibility
  },
  data: () => ({
    showBackdrop: true,
    showActivities: true,
    showActivityLabels: true,
    showActivityShadows: true,
    showEdges: true
  }),
};
</script>
