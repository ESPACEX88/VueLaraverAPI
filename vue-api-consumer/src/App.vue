<template>
  <div class="app-container">
    <header class="header">
      <h1>Sistema de Usuarios</h1>
    </header>
    <section class="content">
      <router-view></router-view>
    </section>
  </div>
</template>

<script>
export default {
  name: "App",
  mounted() {
    
    document.title = "Sistema de Usuarios";
  },
};
</script>

<style scoped>

.app-container {
  font-family: Arial, sans-serif;
  color: #333;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.header {
  background-color: #2c3e50;
  color: #ecf0f1;
  padding: 15px;
  text-align: center;
}

.header h1 {
  margin: 0;
  font-size: 1.8em;
}

/* Content Section */
.content {
  padding: 20px;
  min-height: calc(100vh - 60px);
  background-color: #f9f9f9;
}
</style>
