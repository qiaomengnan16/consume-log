<template>
    <div class="layout-wrapper" :class="classPrefix && `${prefix}-wrapper`">
        <div class="content" :class="classPrefix && `${classPrefix}-content`">
            <slot></slot>
        </div>
        <Nav/>
    </div>
</template>

<script lang="ts">

    import label from '@/assets/icons/label.svg'
    import money from '@/assets/icons/money.svg'
    import statistics from '@/assets/icons/statistics.svg'

    console.log(label)
    console.log(money)
    console.log(statistics)

    export default {
        props: ["classPrefix"],
        name: "Layout"
    }
</script>

<style lang="scss" scoped>

    .layout-wrapper {
        display: flex;
        flex-direction: column;
        height: 100vh;
    }

    .content {
        overflow: auto;
        flex-grow: 1;
    }

</style>