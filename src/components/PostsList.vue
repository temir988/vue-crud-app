<template>
  <ul class="post-list">
    <li class="post" v-for="post in collection" :key="post.id">
      <h2 class="post__title">{{post.title}}</h2>
      <p
        class="post__desc"
      >{{post.description}}</p>
      <footer class="post__footer">
        <div class="post__buttons">
          <button class="button button_update" @click="updatePost(post.id)">Update</button>
          <button class="button button_delete" @click="deletePost(post.id)">Delete</button>
        </div>
      </footer>
    </li>
  </ul>
</template>

<script>
export default {
  props: ['collection', 'setPage', 'deletePost', 'updatePost'],
}
</script>

<style lang="scss" scoped>
  .post-list {
    margin-bottom: 40px;
  }
  .post {
    display: block;
    padding: 40px;
    margin-bottom: 50px;
    background-color: #fff;
    border-radius: 15px;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    &:last-child {
      margin-bottom: 0;
    }
  }
  .post__title {
    border-bottom: 1px solid #eee;
    padding-bottom: 8px;
    margin-bottom: 20px;
  }
  .post__footer {
    display: flex;
  }
  .post__buttons {
    margin-left: auto;
  }
  .button_delete {
    margin-left: 15px;  
  }
</style>


