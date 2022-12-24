<template>
    <div :key="tweet.tweet_id" v-for="tweet in tweets">
        <Tweet :tweet="tweet" />
    </div>
</template>

<script>
import Tweet from "./Tweet"
export default {
    name: "Tweets",
    props: {
        tweets: Array,
    },
    components: {
        Tweet
    }
}
</script>