<template>
    <Layout class-prefix="layout">
        <NumberPad></NumberPad>
        <Types></Types>
        <Notes></Notes>
        <Tag></Tag>
    </Layout>
</template>

<script lang="ts">
    import NumberPad from "@/components/NumberPad.vue";
    import Tag from "@/components/Tag.vue";
    import Notes from "@/components/Notes.vue";
    import Types from "@/components/Types.vue";
    export default {
        name: "Money",
        components: {Types, Notes, Tag, NumberPad}
    }
</script>


<style lang="scss">
    .layout-content {
        display: flex;
        flex-direction: column-reverse;
    }
</style>

<style lang="scss" scoped>

    @import "~@/assets/style/helper.scss";

</style>