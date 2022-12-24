<template>
    <img v-if="media.includes('pbs.twimg.com')" :src="media" />
    <video v-if="media.includes('video.twimg.com')" controls>
        <source :src="media" type="video/mp4">
    </video>
</template>

<script>
export default {
    name: "Media",
    props: {
        media: String
    }
}
</script>

<style scoped>
img {
    width: 100%;
    border-radius: 10px;
    border: 2px solid #555;
}

video {
    width: 100%;
    border-radius: 10px;
    border: 2px solid #555;
}
</style>