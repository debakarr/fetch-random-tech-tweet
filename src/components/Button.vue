<template>
    <button :style="{ background: color }" class="btn"><i :class="icon"></i> {{ text }}</button>
</template>

<script>
export default {
    name: "Button",
    props: {
        text: String,
        color: String,
        icon: String,
    }
}
</script>