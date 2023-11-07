<template>
  <div>
    <h6 class="sidebar-heading d-flex px-3 mt-4 mb-3 text-muted">
      Layers Visibility
    </h6>

    <ul class="list-group">
      <li class="list-group-item">
        <label class="form-check-label form-check form-switch">
          <input class="form-check-input" type="checkbox" v-model="showBackdrop" @click="$emit('showBackdrop', !this.showBackdrop)">
          Backdrop layer
        </label>
      </li>
      <li class="list-group-item">
        <label class="form-check-label form-check form-switch">
          <input class="form-check-input" type="checkbox" v-model="showActivities" @click="$emit('showActivities', !this.showActivities)">
          Activity pictograms
        </label>
      </li>
      <li class="list-group-item">
        <label class="form-check-label form-check form-switch">
          <input class="form-check-input" :disabled="!this.showActivities" type="checkbox" v-model="showActivityShadows" @click="$emit('showActivityShadows', !this.showActivityShadows)">
          Activity shadows
        </label>
      </li>
      <li class="list-group-item">
        <label class="form-check-label form-check form-switch">
          <input class="form-check-input" type="checkbox" v-model="showActivityLabels" @click="$emit('showActivityLabels', !this.showActivityLabels)">
          Activity labels
        </label>
      </li>
      <li class="list-group-item">
        <label class="form-check-label form-check form-switch">
          <input class="form-check-input" type="checkbox" v-model="showEdges" @click="$emit('showEdges', !this.showEdges)">
          Edges
        </label>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "SidebarVisibility",
  data: () => ({
    showBackdrop: true,
    showActivities: true,
    showActivityLabels: true,
    showActivityShadows: true,
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