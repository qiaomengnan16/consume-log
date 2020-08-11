<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style lang="scss">

  @import "~@/assets/style/helper.scss";
  @import "~@/assets/style/reset.scss";

  body {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    line-height: 1.5;
    font-family: $font-hei;
  }

  .clearfix::after {
    content: '';
    display: block;
    clear:both;
  }


  #nav {
    padding: 30px;

    a {
      font-weight: bold;
      color: #2c3e50;

      &.router-link-exact-active {
        color: #42b983;
      }
    }
  }



</style>
