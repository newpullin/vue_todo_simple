<template>
    <header>
        <h1 :class="{'night':toggle_value}">TODO it!</h1>
    </header>
</template>


<script>
export default {
    props: ['toggle_value'],
}
</script>

<style scoped>
    h1 {
        color: #2F3B52;
        font-weight: 900;
        margin: 2.5rem 0 1.5rem;
    }
    h1.night {
        color: #FFFFFF;
    }

</style>