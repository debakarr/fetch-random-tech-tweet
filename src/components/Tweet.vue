<template>
    <div class="tweet">
        <h3 v-html="tweet.tweet_text"></h3>
        <Medias :medias="medias" />
    </div>
</template>

<script>
import Medias from "./Medias"

export default {
    name: "Tweet",
    props: {
        tweet: Object
    },
    components: {
        Medias
    },
    data() {
        return {
            medias: []
        }
    },
    created() {
        this.medias = JSON.parse(this.tweet.tweet_media)
    }
}
</script>

<style scoped>
.tweet {
    background: #f4f4f4;
    margin: 5px;
    padding: 10px 20px;
    cursor: pointer;
}

.tweet h3 {
    display: flex;
    align-items: center;
    justify-content: space-between;
    white-space: pre-wrap;
}
</style>