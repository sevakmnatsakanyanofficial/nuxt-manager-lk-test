<template>
    <div class="notice" data-color="light-purple">
        <slot />
    </div>
</template>

<style lang="scss">
.notice {
    --color_notice: var(--bg_btn_active_chart_info_purple);
    color: var(--color_notice);
    background-color: var(--color_secondary_8);
    padding: 15px;
    border-radius: 3px;
    text-align: center;
}
</style>