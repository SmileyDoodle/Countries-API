<template>
  <header>
      <h1 class="has-text-weight-bold">Where in the world?</h1>
      <div class="dark-mode">
          <i class="far fa-moon"></i>
          <button class="dark-button">Dark Mode</button>
      </div>
  </header>
</template>

<script>
export default {
    name: 'Header',
}
</script>

<style>
header {
    widows: 100%;
    display: flex;
    height: 75px;
    justify-content: space-between;
    align-items: center;
    padding: 0 1.5rem;
}
.dark-mode {
    display: flex;
    justify-content: center;
    align-items: center;
}
.dark-button {
    border: none;
    background-color: transparent;
    font-size: 0.9rem;
}
.dark-button:focus {
    outline: none;
}

@media only screen and (min-width: 1024px) {
    header {
        padding: 0 3rem;
        margin-top: 2rem;
    }
}

</style>