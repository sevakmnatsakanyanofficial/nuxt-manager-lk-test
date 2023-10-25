<template>
    <article class="block">
        <slot />
    </article>
</template>

<style lang="scss">
.block {
    padding: 15px;
    border: .5px solid silver;
    border-radius: 3px;
    width: 100%;
    position: relative;
}
</style>