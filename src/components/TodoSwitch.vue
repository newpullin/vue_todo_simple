<template>
    <div v-bind:class="{'toggle-box' : true}">
        <input type="checkbox" name="checkbox1" id="toggle-box-checkbox" @change = "changing" />
        <label for="toggle-box-checkbox" class="toggle-box-label-left"></label>
        <label for="toggle-box-checkbox" class="toggle-box-label"></label>
    </div>
</template>

<script>
export default {
    methods: {
        changing() {
             this.$emit('changed')
        }
    }
}
</script>

<style scoped>
    .toggle-box {
        position: absolute;
        right: 10px;
        top:10px;
        display: block;
    }
</style>