<template>
    <div class="alert alert-warning alert-dismissible" role="alert">
        <button type="button" class="close" data-dismiss="alert" aria-label="Close" @click="close()">
            <span aria-hidden="true">&times;</span>
        </button>
        <strong>
            <slot name="msg"></slot>
        </strong>
    </div>
</template>
<script>
export default {
    methods: {
        close() {
            this.$emit('alert-close')
        }
    }
}
</script>
<style>

</style>
