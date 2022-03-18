<template>
  <h2>{{ this.id }}</h2>
  <div class="entire-post">
    <div class="cardDetail">
      <!--      <h1>TITLE</h1>-->
      <h2 id="titleH1" class="titleCard" v-on:click="updateTitle">{{ card.title.rendered }} </h2>


      <form class="titleUpdateForm" @submit="onSubmitP">
        <input
            id="inputH1"
            class="input-title"
            placeholder="Modify your title"
            name="newTC"
            v-model="newTC"
            v-on:keyup.enter="recoverTitle"
        />

      </form>

      <h3>Description</h3>
      <div id="descH1" class="description" v-html="card.content.rendered" v-on:click="updateDescription">
      </div>
      <div>
        <form @submit="onSubmitPDesc">
          <input
              id="inputDesc"
              class="input-description"
              placeholder="Modify your description"
              name="newDescC"
              v-model="newDescC"
              v-on:keyup.enter="recoverDescription"
          />

        </form>

      </div>
      <h4>Comments</h4>
      <div v-for="comment in comments" v-bind:key="comment.id">
        <div @dblclick="handleCommentUpdate" class="commentCSS" v-if="comment.post == this.id">
          <div class="commentUpdate" v-if="showUpdateComment" v-html="comment.content.rendered"/>
          <form @submit="onSubmitUComment" v-if="!showUpdateComment" class="updateCommentCForm">
            <input name="commentUC" v-model="commentUC" type="text" autofocus
                   :placeholder.prop="comment.content.rendered" minlength="2">
            <input type="submit" class="updateCommentFormButtonUpdate" value="update">
            <button class="updateCommentFormButtonClose" @click="handleUpdateCommentFormButtonCancel">Cancel</button>
          </form>
          <div class="buttonEachComment">
            <button class="commentDelete" @click="handleDeleteComment">Delete comment</button>
          </div>
        </div>
        <!--  <comments></comments>  -->
      </div>
      <form @submit="onSubmitC" v-if="!showAddComment" class="addCommentForm">
        <input name="titleC" v-model="titleC" type="text" autofocus placeholder="Write the comment"
               minlength="2">
        <input type="submit" class="addCommentFormButtonAdd" value="Add a comment">
        <button class="addCommentFormButtonClose" @click="handleAddCommentFormButtonCancel">Cancel</button>
      </form>

      <div class="buttonComment">
        <router-link class="buttonCSS" to="/">
          <button class="button-return">Return</button>
        </router-link>
        <button class="commentAdd" v-if="showAddComment" @click="handleAddComment">Add comment</button>
        <router-link class="buttonCSS" to="/">
          <button class="button-delete" v-on:click="deletePost">Delete Card</button>
        </router-link>
      </div>
    </div>


  </div>
</template>

<script>
//import axios from "axios";
// import comments from "./comments";
import {mapActions} from "vuex";
// import comments from "./comments";
/*const LOCAL = "http://localhost:81/";
const URL = "wordpress//wp-json/wp/v2/";*/
export default {
  name: "card",
  // components: {comments},
  props: ["id"],
  data() {
    return {
      newTC: "",
      newDescC: "",
      titleC: "",
      showUpdateComment: true,
      showAddComment: true,
    }
  },
  created() {
    this.$store.dispatch("SET_CURRENT_POST", this.id);
    this.$store.dispatch("getComments");
  },
  computed: {
    card: function () {
      return this.$store.getters.getCurrentPost;
    },
    comments() {
      return this.$store.state.comments;
    },
  },
  methods: {
    //...mapActions(["removePost","updatePost"]),
    ...mapActions(["updatePost", "removePost", "addComment", "removeComment"]),
    deletePost() {
      console.log("delete");
      this.removePost(this.card.id);
    },
    onSubmitP(e) {
      e.preventDefault();
      console.log("update");
      const card = {
        id: this.id,
        title: this.newTC,
      };
      console.log(card);
      this.updatePost(card);

    },
    onSubmitPDesc(e) {
      e.preventDefault();
      console.log("update");
      const card = {
        id: this.id,
        content: this.newDescC,
      };
      console.log(card);
      this.updatePost(card);

    },

    handleCommentUpdate() {
      // let showUpdateComment.post.id = false;
      // showUpdateComment.post.id = true
      this.showUpdateComment = !this.showUpdateComment
    },

    handleUpdateCommentFormButtonCancel() {
      this.showUpdateComment = !this.showUpdateComment
    },

    handleAddComment() {
      this.showAddComment = !this.showAddComment
    },

    onSubmitC(e) {
      e.preventDefault();
      const comment = {"content": this.titleC, post: this.id, status: "approved"}
      // console.log(post);
      this.addComment(comment);
      this.showAddComment = !this.showAddComment
      this.titleC = ""
    },

    handleAddCommentFormButtonCancel() {
      this.showAddComment = !this.showAddComment
    },

    handleDeleteComment() {
      console.log("delete comment")
      // const comment = {
      //   post: this.id,
      // };
      // this.removeComment(this.id)
      // this.removeComment(this.card.id)
      // this.removeComment(this.comments.id)
      // this.removeComment(this.comment.id)
      // this.removeComment(this.post.id)
      // this.removeComment(comment)
      // this.removeComment(comment.post)
    },

    updateTitle() {
      let divTitle = document.getElementById('inputH1');
      let h1 = document.getElementById("titleH1");
      if (divTitle.style.display === "none") {
        divTitle.style.display = "block";
        h1.style.display = "none";
      } else {
        divTitle.style.display = "none";
      }
    },
    recoverTitle() {
      let divTitle = document.getElementById('inputH1');
      let h1 = document.getElementById("titleH1");
      if (h1.style.display === "none") {
        h1.style.display = "block";
        divTitle.style.display = "none";
      } else {
        h1.style.display = "none";
      }
    },
    updateDescription() {
      let divDesc = document.getElementById("inputDesc");
      let desc = document.getElementById("descH1");
      console.log(divDesc);
      console.log(desc);
      if (divDesc.style.display === "none") {
        divDesc.style.display = "block";
        desc.style.display = "none";
      } else {
        divDesc.style.display = "none";
      }
    },
    recoverDescription() {
      let divDesc = document.getElementById("inputDesc");
      let desc = document.getElementById("descH1");
      if (divDesc.style.display === "block") {
        divDesc.style.display = "none";
        desc.style.display = "block";
      } else {
        divDesc.style.display = "none";
      }
    },
  },
};
</script>

<style>

h3 {
  font-size: 20px;
}

h4 {
  font-size: 20px;
}

.titleCard {
  font-size: 35px;
  margin: auto;
  padding-top: 20px;
}

.updateCommentCForm {
  display: flex;
  width: 90%;
  flex-wrap: wrap;
}

[name="commentUC"] {
  display: flex;
  width: 100%;
  margin-bottom: 8px;
}

.updateCommentFormButtonUpdate {
  display: flex;
  margin-right: auto;
  border-radius: 3px;
}

.updateCommentFormButtonClose {
  display: flex;
  margin-left: auto;
  border-radius: 3px;
}

.buttonEachComment {
  display: flex;
  width: 90%;
  justify-content: space-evenly;
}

.commentDelete {
  border-radius: 3px;
  margin-left: auto;
  margin-bottom: 8px;
  margin-top: 8px;
}

.commentAdd {
  border-radius: 3px;
  margin-bottom: 8px;
  margin-top: 8px;
}

.addCommentForm {
  display: flex;
  width: 90%;
  flex-wrap: wrap;
  align-self: center;
  margin-top: 20px;
  margin-bottom: 20px;
}

[name="titleC"] {
  display: flex;
  width: 100%;
  margin-bottom: 8px;
}

.addCommentFormButtonAdd {
  display: flex;
  margin-right: auto;
  border-radius: 3px;
}

.addCommentFormButtonClose {
  display: flex;
  margin-left: auto;
  border-radius: 3px;
}

.commentCSS {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.commentUpdate {
  display: flex;
  width: 90%;
  height: fit-content;
  text-align: left;
  padding: 5px;
  background-color: #bee5eb;
  border-radius: 3px;
}

.buttonComment {
  display: flex;
  /*flex-wrap: wrap;*/
  width: 90%;
  margin: auto;
  align-items: center;
  /*justify-content: center;*/
  justify-content: space-between;
  /*margin-top: 5px;*/
  /*margin-bottom: 8px;*/
  padding: 8px;
}

.buttonCSS {
  text-decoration: none;
}

.button-delete {
  display: flex;
  /*justify-content: right;*/
  border-radius: 3px;
}

.button-return {
  display: flex;
  /*margin-left: 13px;*/
  border-radius: 3px;
}

.entire-post {
  display: flex;
  justify-content: center;
  position: relative;
}

.cardDetail {
  position: absolute;
  width: 500px;
  max-width: 500px;
  height: fit-content;
  background-color: #ebecf0;
  color: #172b4d;
  border-radius: 5%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  flex-wrap: nowrap;
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.left-column {
  width: 90%;
}
.top-left {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.description {
  display: flex;
  margin: auto;
  width: 90%;
  text-align: left;
  min-height: 100px;
  height: fit-content;
}

.titleUpdateForm {
  display: flex;
  justify-content: center;
}

.input-title {
  display: none;
  height: 37px;
  width: 90%;
  justify-items: center;
  margin-top: 21px;
  margin-bottom: 15px;
  margin-left: 5px;
  font-size: 32px;
}
.input-description {
  display: none;
  margin: auto;
  height: 200px;
  width: 90%;
  border: 1px solid red;
}
.right-column {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
}
</style>