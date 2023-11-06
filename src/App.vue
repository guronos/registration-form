<template>
    <div id="app">
        <RegistrationForm />
    </div>
</template>

<script>
import RegistrationForm from '@/components/RegistrationForm.vue'

export default {
    name: 'App',
    components: {
        RegistrationForm,
    },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700&display=swap');

* {
    margin: 0;
    padding: 0;
}

*,
*::before,
*::after {
    box-sizing: border-box;
}

#app {
    font-family: 'Montserrat', sans-serif;
}
</style>
