<template>
    <div v-for="media in medias">
        <Media :media="media" />
    </div>
</template>

<script>
import Media from "./Media"

export default {
    name: "Medias",
    props: {
        medias: Array,
    },
    components: {
        Media
    }
}
</script>