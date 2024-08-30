<template>
  <main>
    <Navbar />
    <SidebarData
      :configuration="configuration"
      :dfg="dfg"
      @configuration="this.configuration = $event.config; this.dfg = $event.dfg"
      />
    <div class="container">
      <div class="row" v-show="this.configuration != null && this.dfg != null">
        <div class="col-md-3 bg-light sidebar">
          <SidebarVisibility
            @showBackdrop="this.showBackdrop = $event"
            @showActivities="this.showActivities = $event"
            @showActivityLabels="this.showActivityLabels = $event"
            @showActivityShadows="this.showActivityShadows = $event"
            @showEdges="this.showEdges = $event"
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
              :configuration="configuration"
              :dfg="dfg"
          />
        </div>
      </div>
      <div class="row" v-if="configuration == null || dfg == null">
        <div class="col-12 text-muted text-center" style="height: calc(100vh - 3.5rem); padding-top: 25vh">
          <p class="fs-5"><em>To start the visualization it is necessary to provide the configuration data.</em></p>
          <p><a class="btn btn-primary" data-bs-toggle="offcanvas" href="#ConfigureData">Configure data</a></p>
          <p style="margin-top: 50px;"><img src="/tiramisu.png" /></p>
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
    showActivityShadows: false,
    showEdges: true,
    configuration: null,
    dfg: null
  }),
};
</script>

<script setup>
</script>