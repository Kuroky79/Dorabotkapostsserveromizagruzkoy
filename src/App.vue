<template>
  <div class="app">
    <h2>Страница с постами</h2>
    <my-button @click="showDialog" style="margin: 13px 0;">
      Созать пользователя
    </my-button>
    <my-dialog v-model:show="dialogVisible" >
    <post-form
        @create="createPost"
    />
    </my-dialog>
    <post-list
        :posts="posts"
        @remove = removePost
        v-if="!isPostsLoading"
    />
    <div v-else>Идет загрузка...</div>
  </div>
</template>

<script>
import PostForm from "./components/PostForm"
import PostList from "@/components/PostList"
import MyDialog from "@/components/UI/MyDialog";
import MyButton from "@/components/UI/MyButton";
import axios from 'axios'
export default {
  components:{
    MyButton,
    MyDialog,
    PostForm, PostList
  },
  data(){
    return {
      posts: [],
      dialogVisible: false,
      isPostsLoading: false,
    }

    },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true
    },
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        setTimeout(async () => {
          const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10');
          this.posts = response.data;
          this.isPostsLoading = false;
        }, 1000)
      } catch (e) {
        alert('Ошибка')
      } finally {
        this.isPostsLoading = false;
      }
    }
  },
  mounted() {
    this.fetchPosts()
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.app{
  padding: 20px;
}
form {
  display: flex;
  flex-direction: column;
}

</style>