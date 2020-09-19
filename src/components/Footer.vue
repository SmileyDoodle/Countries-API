<template>
  <footer class="footer">
      <p>© {{ new Date().getFullYear() }} - SmileyDoodle</p>
  </footer>
</template>

<script>
export default {
  name: 'Footer',
}
</script>

<style>
.footer {
    background-color: var(--bg-color);
    padding-bottom: 1rem;
}
.footer p {
    font-size: 0.7rem;
}
</style>