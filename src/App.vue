<template>
  <main>
    <Navbar />
    <div class="container">
      <div class="row">
        <div class="col-md-3 bg-light sidebar">
          <h6 class="sidebar-heading d-flex px-3 mt-4 mb-3 text-muted">
            Configuration
          </h6>

          <ul class="list-group">
            <li class="list-group-item">
              <label class="form-check-label form-check form-switch">
                <input class="form-check-input" type="checkbox" v-model="showBackdrop">
                Backdrop layer
              </label>
            </li>
            <li class="list-group-item">
              <label class="form-check-label form-check form-switch">
                <input class="form-check-input" type="checkbox" v-model="showActivities">
                Activity pictograms layer
              </label>
            </li>
            <li class="list-group-item">
              <label class="form-check-label form-check form-switch">
                <input class="form-check-input" type="checkbox" v-model="showActivityLabels">
                Activity labels layer
              </label>
            </li>
            <li class="list-group-item">
              <label class="form-check-label form-check form-switch">
                <input class="form-check-input" type="checkbox" v-model="showEdges">
                Edges layer
              </label>
            </li>
          </ul>
        </div>
        <div class="col-md-9 ps-3">
          <h3 class="py-3 mb-3 border-bottom">Visualization</h3>
          <Tiramisu
              :show-backdrop="showBackdrop"
              :show-activities="showActivities"
              :show-activity-labels="showActivityLabels"
              :show-edges="showEdges" />
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import Tiramisu from "./components/Tiramisu.vue";

export default {
  name: "App",
  components: {
    Navbar,
    Tiramisu,
  },
  data: () => ({
    showBackdrop: true,
    showActivities: true,
    showActivityLabels: true,
    showEdges: true
  }),
};
</script>

<style>
.sidebar {
  height: calc(100vh - 3.5rem);
  overflow-y: auto;
}
</style>