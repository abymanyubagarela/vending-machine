<template>
<VendingMachine />
</template>

<script>
import VendingMachine from './components/VendingMachine.vue';

export default {
    name: 'App',
    components: {
        VendingMachine,
    },
};
</script>

<style>
body {
    padding: 0;
    margin: 0;
    background-color: #fafafa;
}
</style>
