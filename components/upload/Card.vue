<template>
    <UploadBlock class="card">
        <slot />
    </UploadBlock>
</template>

<style lang="scss">
.card {
    border-left-width: 10px !important;
    border-radius: 3px;
    font-size: 14px;
    margin-top: 15px;
}

.color_success {
    border-left-color: green !important;
}

.color_danger {
    border-left-color: red !important;
}
</style>