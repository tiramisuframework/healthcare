<template>
  <nav class="navbar navbar-expand-lg bg-body-tertiary shadow" data-bs-theme="dark" style="background-color: #A2035F !important">
    <div class="container-fluid">
      <img src="/tiramisu.png" height="40" style="margin-right: 10px" />
      <a class="navbar-brand" href="#"><em>Berries</em> Tiramisu Visualization</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link" data-bs-toggle="offcanvas" href="#ConfigureData">Configure data</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Navbar",
};
</script>