<script>
import TopMenu from './components/TopMenu.vue'
import CupsInput from './components/CupsInput.vue';

export default {
    name: "App",
    components: {
        TopMenu,
        CupsInput
    }
}

</script>

<template>
    <div>
        <TopMenu/>
        <CupsInput/>
    </div>
</template> 

<style>


</style>
