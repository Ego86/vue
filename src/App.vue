<template>
  <header>
    <posts-select v-model="selectedSort" :options="sortOptions"></posts-select>
  </header>
  <main>
    <post-list :posts="posts" :error="error" :isLoading="isLoading"></post-list>
  </main>
  <footer style="display: flex; flex-wrap:wrap;">
      <button 
      v-for="(count, index) in pagesCount"
      v-show="page <= 6"
      v-memo="pagesCount"
      @click="handlerPage"
      :key="index" 
      >
      {{ count }}
    </button>
    </footer>
</template>

<script setup>
import { ref, watch, onMounted } from "vue"
import PostList from "@/components/Posts/PostsList"
import PostsSelect from "@/components/Posts/PostsSelect"
let posts = ref([]);
let page = ref(1);
let pagesCount = 0;
const error = ref([]);
let isLoading = ref(false);
const selectedSort = ref("")
const sortOptions = ref([
  { value: "name", name: "По имени" },
  { value: "status", name: "По статусу" }
])

function handlerPage(event) {
  event.preventDefault();
  page.value = Number(event.target.innerText)
}
async function handlerPosts(numberPage) {
  isLoading = true;
  try {
    const resp = await fetch(`https://rickandmortyapi.com/api/character?page=${numberPage}`)
    const data = await resp.json()
    const result = data.results
    pagesCount = data.info.pages
    posts.value = result
  } catch (err) {
    error.value.push(err)
  } finally {
    isLoading = false
  }
}

onMounted(() => {
  handlerPosts(page.value);
});
watch(page, (newValue) => handlerPosts(newValue))

watch(selectedSort, (newValue) => {
  posts.value.sort((post1, post2) => {
    return post1[newValue].localeCompare(post2[newValue])
  })
})

</script>

<style>
main{
  display:flex;
  flex-wrap: wrap;
}
select {
  background-color: #fff;
  padding: 5px 8px;
  border: solid 2px rgb(60, 62, 68);
  color: rgb(60, 62, 68);
  margin-right: 8px;
  margin-bottom: 3px;
}
button{
    cursor: pointer;
  background-color: #fff;
  padding: 5px 8px;
  border: solid 2px rgb(60, 62, 68);
  color: rgb(60, 62, 68);
  margin-right: 8px;
  margin-bottom: 3px;
  border-radius: 3px;
}

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

</style>
