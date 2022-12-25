<template>
    <header>
        <h1>{{ title }}</h1>
    </header>
    <Button @click="$emit('go-to-previous-tweet')" text="Prev" color="#337ab7" icon="fa-solid fa-backward" />
    <Button v-if="this.$parent.isFetching" text="Fetching" color="#5cb85c" icon="fa-solid fa-spinner fa-spin" />
    <Button v-else @click="$emit('fetch-random-tweet')" text="Fetch tweet" color="#5cb85c"
        icon="fa-solid fa-magnifying-glass" />
    <Button @click="$emit('go-to-next-tweet')" text="Next" color="#f0ad4e" icon="fa-solid fa-forward" />
    <Button @click="openTweetInNewTab" text="Tweet Source" color="#5bc0de" icon="fa-brands fa-twitter" />
    <Button @click="$emit('download-as-pdf')" text="Download as PDF" color="#d9534f" icon="fa-solid fa-download" />
</template>

<script>
import Button from "./Button"

export default {
    name: "Header",
    props: {
        title: String,
        tweet: Object,
    },
    components: {
        Button
    },
    methods: {
        openTweetInNewTab() {
            const url = "https://twitter.com/" + this.tweet.username + "/status/" + this.tweet.tweet_id
            window.open(url, '_blank');
        }
    }
}
</script>

<style scoped>
header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}
</style>