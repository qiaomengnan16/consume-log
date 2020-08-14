<template>
  <svg class="icon">
    <use :xlink:href="`#${name}`"></use>
  </svg>
</template>

<script lang="ts">
  const importAll = (requireContext: __WebpackModuleApi.RequireContext) => requireContext.keys().forEach(requireContext);

  try {
    importAll(require.context('../assets/icon', true, /\.svg$/));
  } catch (error) {
    console.log(error);
  }
  export default {
    name: 'Icon',
    props: ['name']
  };
</script>

<style scoped lang="scss">
  .icon {
    height: 1em;
    width: 1em;
    vertical-align: -.15em;
    fill: currentColor;
    overflow: hidden;
  }
</style>