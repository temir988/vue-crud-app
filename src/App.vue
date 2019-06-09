<template>
  <div id="app">
    <h1 class="title">{{ msg }}</h1>

    <header class="header">
      <h2 href="/" class="logo">Amazing posts</h2>
      <div class="posts-per-page">
        Posts per page
        <input class="posts-per-page__input" v-model="postsPerPage" @change="setPage(1)">
      </div>
    </header>
    
    <PostsList
      :collection="collection"
      :setPage="setPage"
      :updatePost="updatePost"
      :deletePost="deletePost"
    ></PostsList>

    <div class="create">
      <button class="button button_create" @click="modalCreate.show = true">Create</button>
    </div>
    
    <Modal
      :modalData="modalCreate"
      :posts="posts"
      :setPage="setPage"
    ></Modal>
    <Modal
      :modalData="modalUpdate"
      :posts="posts"
    ></Modal>

    <div class="pagination">
      <button 
        v-for="page in pagination.pages" 
        :key="page" 
        class="button"
        :class="{active: pagination.currentPage === page}"
        @click="setPage(page)"
      >
        {{ page }}
      </button>
    </div>
  </div>
</template>

<script>
import Modal from './components/Modal';
import PostsList from './components/PostsList';

export default {
  components: {
    Modal,
    PostsList
  },
  name: "app",
  data() {
    return {
      msg: "Hello world!",
      postsPerPage: 2,
      pagination: {},
      modalCreate: {
        type: 'create',
        title: 'Create new post',
        show: false,
        postTitle: '',
        PostDescription: '',
      },
      modalUpdate: {
        type: 'update',
        title: 'Update post',
        show: false,
        postID: 0,
        postTitle: '',
        PostDescription: '',
      },    
      posts: [
        {
          id: 1,
          title: 'Post title 1',
          description: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Ipsa deserunt hic veritatis perspiciatis ad repudiandae, aperiam harum molestias impedit eos! Vero ab blanditiis excepturi veniam veritatis nihil quam recusandae quidem.'
        },
        {
          id: 2,
          title: 'Post title 2',
          description: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Ipsa deserunt hic veritatis perspiciatis ad repudiandae, aperiam harum molestias impedit eos! Vero ab blanditiis excepturi veniam veritatis nihil quam recusandae quidem.'
        },
        {
          id: 3,
          title: 'Post title 3',
          description: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Ipsa deserunt hic veritatis perspiciatis ad repudiandae, aperiam harum molestias impedit eos! Vero ab blanditiis excepturi veniam veritatis nihil quam recusandae quidem.'
        },
        {
          id: 4,
          title: 'Post title 4',
          description: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Ipsa deserunt hic veritatis perspiciatis ad repudiandae, aperiam harum molestias impedit eos! Vero ab blanditiis excepturi veniam veritatis nihil quam recusandae quidem.'
        }
      ]
    };
  },
  computed: {
    collection() {
      return this.paginate(this.posts);
    }
  },
  methods: {
    deletePost (id) {
      const post = this.posts.find((el) => {
        return el.id === id;
      });
      const postIndex = this.posts.indexOf(post);
      this.posts.splice(postIndex, 1);
      this.setPage(1);
    },
    updatePost(id) {
      const post = this.posts.find((el) => {
        return el.id === id;
      });
      const postIndex = this.posts.indexOf(post);

      this.modalUpdate.postID = post.id;
      this.modalUpdate.postTitle = post.title;
      this.modalUpdate.postDescription = post.description;
      this.modalUpdate.show = true;
    },
    setPage(page) {
      this.pagination = this.paginator(this.posts.length, page);
    },
    paginate(posts) {
      return posts.slice(
        this.pagination.startIndex,
        this.pagination.endIndex + 1
      );
    },
    paginator(totalItems, currentPage) {
      const startIndex = (currentPage - 1) * this.postsPerPage;
      const endIndex = startIndex + +this.postsPerPage - 1;
      return {
        currentPage,
        startIndex,
        endIndex,
        pages: this.range(Math.ceil(totalItems / this.postsPerPage) + 1)
      };
    },
    range(amount) {
      const result = [];
      let i = 1;
      while (i < amount) {
        result.push(i);
        i++;
      }
      return result;
    }
  },
  created() {
    this.setPage(1);
  }
};
</script>

<style lang="scss">
  @import './assets/scss/main.scss';
</style>
