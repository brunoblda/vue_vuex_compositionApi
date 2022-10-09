<template>
  <p>{{count}}</p>
  <button @click="click">Increment</button>
  <p></p>
  <button 
    v-for="post in posts"
    :key="post.id"
    @click="click2(post)"
  >
    {{post.title}}
  </button>
<p></p>
  <button 
    v-for="post in posts2"
    :key="post.id"
    @click="click3(post)"
  >
    {{post.title}}
  </button>

  {{postId}}

  <div
    v-if="currentPost"
  >
    <h2>{{currentPost.title}}</h2>
    <h4>{{currentPost.content}}</h4>

  </div>

</template>

<script lang="ts">
import { computed, defineComponent, onMounted } from 'vue';
import { useStore } from 'vuex';

export default defineComponent({
  name: 'App',
  setup(){

    const posts = [
      { id: 1, title: 'Post #1'},
      { id: 2, title: 'Post #2'},
    ]

    const store = useStore() 

    const click = () => {
      store.commit('posts/increment', 10)
    }

    const click2 = (post: any) => {
      store.commit('posts/setPostId', post.id)
    }

    const click3 = (post: any) => {
      store.commit('posts/setPostId', post.id)
    }

    console.log(store)
    console.log(store.state.posts.count)

    const fetchData = () => {
      store.dispatch('posts/fetch', 'Post')
    }

    onMounted(()=> {
      fetchData()
    }) 

    return {
      postId: computed(() => store.state.posts.postId),
      count: computed(() => store.state.posts.count),
      click,
      posts,
      click2,
      fetchData,
      click3,
      posts2 : computed(() => store.state.posts.all),
      currentPost: computed(() => store.getters['posts/currentPost'])
    }
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
