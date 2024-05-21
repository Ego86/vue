<template>
    <p v-if="isLoading">Loading...</p>
    <section class="container-post" v-else v-for="post in posts" :key="post.id">
        <img :src="post.image" alt="" />
        <article class="post-title">
            <article>
                <h2 class="post-title__links">{{ post.name }}</h2>
                <p>{{ post.status }} - {{ post.species }}</p>
            </article>
            <article>
                <p class="post-title__gray">Last known location:</p>
                <p class="post-title__links">{{ post.origin.name }}</p>
            </article>
            <article>
                <p class="post-title__gray">First seen in:</p>
                <p class="post-title__links">{{ post.location.name }}</p>
            </article>
        </article>
    </section>
    <p v-if="error.length > 0">{{ error }}</p>
</template>

<script setup>
import {defineProps} from "vue"
defineProps({
    posts: Array,
    error: Array,
    isLoading: Boolean
})
</script>

<style>
.container-post {
    color: #fff;
    width: 600px;
    height: 220px;
    display: flex;
    overflow: hidden;
    background: rgb(60, 62, 68);
    border-radius: 0.5rem;
    margin: 0.75rem;
    box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px, rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;

    &>img {
        height: 100%;
        margin: 0px;
        opacity: 1;
        transition: opacity 0.5s ease 0s;
        object-position: center center;
        object-fit: cover;
    }

    .post-title {
        flex: 3 1 0%;
        position: relative;
        padding: 0.75rem;
        color: rgb(255, 255, 255);
        display: flex;
        flex-direction: column;

        &>article {
            flex: 1 1 0%;
            display: flex;
            flex-direction: column;
        }
    }

    .post-title__links:hover {
        color: rgb(255, 152, 0);
        cursor: pointer;
    }

    .post-title__gray {
        color: rgb(158, 158, 158);
    }
}
</style>
