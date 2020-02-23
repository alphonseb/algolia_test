<template>
  <header class="header">
    <img src="../assets/f1_logo.svg" alt="F1 Logo" />
    <h1 class="header__title">
      Formula One Race Browser
    </h1>
  </header>
</template>

<script>
export default {
  name: "Header"
};
</script>

<style lang="scss" scoped>
.header {
  padding: 48px 32px;
  background-color: var(--formula-red);
  display: flex;
  align-items: center;

  &__title {
    color: #fff;
    font-size: 3rem;
    font-family: "Formula1-Bold", sans-serif;
    margin-left: 24px;
  }
}
</style>
