<template>
  <div class="modal__wrapper" :class="{active: modalData.show}">
      <div class="modal">
        <div class="modal__header">
          <div class="modal__title">{{ modalData.title }}</div>
        </div>
         <div class="modal__body">
          <form @submit.prevent="onSubmit">
            <div class="modal__form">
              <input type="text" placeholder="Post title" class="input" v-model="modalData.postTitle">
              <textarea placeholder="Post description" class="textarea" v-model="modalData.postDescription"></textarea>
              <div class="modal__butons">                
                <button class="button button_delete" @click.prevent="modalData.show = false">Close</button>
                <button type="submit" class="button button_create">Apply</button>
              </div>
            </div>          
          </form>
        </div> 
      </div>
    </div>
</template>

<script>
export default {
  props: ['modalData', 'posts', 'setPage'],
  data() {
    return {
      postID: 100
    }
  },
  methods: {
    onSubmit() {
      if (this.modalData.type === 'create') {
        this.posts.push({
          id: ++this.postID,
          title: this.modalData.postTitle,
          description: this.modalData.postDescription
        });
        
        //reset
        this.modalData.postTitle = '';
        this.modalData.postDescription = '';
        this.modalData.show = false;
        this.setPage(1);
      } else if (this.modalData.type === 'update') {
        const post = {
          id: this.modalData.postID,
          title: this.modalData.postTitle,
          description: this.modalData.postDescription
        };
        const oldPost = this.posts.find((el) => {
          return el.id === this.modalData.postID;
        });
        const oldPostIndex = this.posts.indexOf(oldPost);
        this.posts.splice(oldPostIndex, 1, post);        

        // reset
        this.modalData.postTitle = '';
        this.modalData.postDescription = '';
        this.modalData.show = false;        
      }      
    }
  }
}
</script>

<style lang="scss" scoped>
  .modal__wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
    background-color: rgba(0,0,0,.75);
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    opacity: 0;
    pointer-events: none;
    transition: opacity .3s;
    &.active {
      opacity: 1;
      pointer-events: auto;
    }
  }
  .modal {
    width: 400px;
    background-color: #fafafa;
    padding: 40px;
    border-radius: 15px;
  }
  .modal__header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 30px;
  }
  .modal__butons {
    display: flex;
    justify-content: space-between;
  }
</style>
