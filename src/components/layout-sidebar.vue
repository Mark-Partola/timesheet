<template>
  <nav class="layout-sidebar">

  </nav>
</template>

<script>
export default {
  name: "layout-sidebar"
};
</script>

<style lang="scss" scoped>
$layout-sidebar-bgc: #42b983;

.layout-sidebar {
  background-color: $layout-sidebar-bgc;
  border-right: 2px solid darken($layout-sidebar-bgc, 10);
  height: 100%;
  transition: width 0.3s;
}
</style>
