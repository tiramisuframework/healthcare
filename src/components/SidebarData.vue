<template>
  <div class="offcanvas offcanvas-start" tabindex="-1" id="ConfigureData" aria-labelledby="offcanvasExampleLabel">
    <div class="offcanvas-header">
      <h5 class="offcanvas-title" id="offcanvasExampleLabel">Configure data sources</h5>
      <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
    </div>
    <div class="offcanvas-body">
      <p class="text-muted">Use the form below to configure the URLs of the JSON file and the URL of the DFG file.</p>
      <form>
        <div class="mb-3">
          <label for="jsonConfiguration" class="form-label">URL of the JSON configuration:</label>
          <input type="text" class="form-control" id="jsonConfiguration" v-model="this.configuration">
        </div>
        <div class="mb-3">
          <label for="dfgFile" class="form-label">URL of the DFG file:</label>
          <input type="text" class="form-control" id="dfgFile" v-model="this.dfg">
        </div>
        <button type="button"
                class="btn btn-primary"
                @click="$emit('configuration', {config: this.configuration, dfg: this.dfg})"
                data-bs-toggle="offcanvas" href="#ConfigureData">
          Submit
        </button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "SidebarData",
  data: () => ({
    configuration: "https://raw.githubusercontent.com/tiramisuframework/healthcare/master/examples/example1.json",
    dfg: "https://raw.githubusercontent.com/tiramisuframework/healthcare/master/examples/example1.dfg"
  }),
};
</script>
