<template>
  <div class="LiveFeed">
    <div class="LiveFeed__post" v-for="item in list" :key="item.id">
      <h2>{{ item.firstname }} {{ item.lastname }}</h2>
      <p>{{ item.text }}</p>
      <div>
        <button v-if="item.userId == $store.state.userId || $store.state.userId == admin" @click="deletePost(item.id)">Supprimer</button> |
        <button class="likeBtn" v-if="item.userLiked && item.userLiked.indexOf($store.state.userId) > -1" @click="likePost(item.id, $store.state.userId)">Like</button>
        <button v-else @click="likePost(item.id, $store.state.userId)">Like</button> | 
        <button @click="item.isHidden = !item.isHidden">Commenter ?</button>
      </div>
      <fieldset v-if="!item.isHidden">
        <legend>Com</legend>
        <textarea type="text" v-model="form.text"/>
        <button v-on:click="submit(item.id, $store.state.userId)">Envoyer</button>
      </fieldset>
      <div class="comments">
        <div class="comments__list" v-for="comItem in item.comments" :key="comItem.id">
          <h2>{{ comItem.firstname }} {{ comItem.lastname }}</h2>
          <p>{{ comItem.text }} </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { mapState } from "vuex";
import checkToken from "@/services/checkToken.js";
import admin from "@/services/admin.js";
import urlVariable from "@/services/urlVariable.js";

export default {
  name: "LiveFeed",
  props: {
    list: {
      type: Array,
    },
  },

  data() {
    return {
      urlAPI: urlVariable.getUrl(),
      admin: admin.getAdminId(),
      form: {
        text: "",
        firstname: this.$store.state.firstname,
        lastname: this.$store.state.lastname,
      },
    };
  },

  computed: {
    ...mapState(["token"]),
  },

  methods: {
    // request for post a comment
    submit(postId, userId) {
      let payload = { postId, userId, post: this.form};
      console.log(payload);
      // import service checkToken
      let token = checkToken.getUserToken(this.$store);
      if (token) {
        const requestOptions = {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            authorization: "Bearer: " + this.token,
          },
          body: JSON.stringify({ payload }),
        };
        fetch(this.urlAPI + "api/posts/comment", requestOptions)
          .then((response) => response.json())
          .then((response) => {
            console.log(response);
            this.callAllPosts()
          })
          .catch((e) => {
            console.log(e);
          });
      } else {
        this.clearStoreAndStorage();
      }
    },

    // request for delete a post
    deletePost(id) {
      console.log(id);
      // import service checkToken
      let token = checkToken.getUserToken(this.$store);
      if (token) {
        axios
          .request({
            method: "delete",
            baseURL: this.urlAPI + "api/posts/" + id,
            headers: {
              Authorization: "Bearer: " + this.$store.state.token,
            },
          })
          .then((response) => {
            console.log(response);
            console.log("Post supprimé !");
            this.callAllPosts();
          })
          .catch((e) => {
            console.log(e);
          });
      } else {
        this.clearStoreAndStorage();
      }
    },

    // request for like a post
    likePost(postId, userId) {
      let payload = { postId, userId };
      console.log(payload);
      // import service checkToken
      let token = checkToken.getUserToken(this.$store);
      if (token) {
        const requestOptions = {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
            authorization: "Bearer: " + this.token,
          },
          body: JSON.stringify({ payload }),
        };
        fetch(this.urlAPI + "api/posts/like", requestOptions)
          .then((response) => response.json())
          .then((response) => {
            console.log(response);
            this.callAllPosts()
          })
          .catch((e) => {
            console.log(e);
          });
      } else {
        this.clearStoreAndStorage();
      }
    },

    // request for all posts
    callAllPosts() {
      axios
        .request({
          method: "get",
          baseURL: this.urlAPI + "api/posts",
          headers: {
            Authorization: "Bearer: " + this.$store.state.token,
          },
        })
        .then((response) => {
          this.list = this.initAction(response.data);
          console.log(this.list);
        })
        .catch((e) => {
          console.log(e);
        });
    },

    // function for clear the store and the storage
    clearStoreAndStorage() {
      localStorage.clear();
      this.$store.commit("cleanStore");
      window.location = "http://localhost:8080/#/";
    },

    // Unicity of each comment module
    initAction(data){
      data.forEach(element => {
        element.isHidden = true;
      });
      return data;
    }
  },

  created() {
    // import service checkToken
    let token = checkToken.getUserToken(this.$store);
    if (token) {
      this.callAllPosts();
    } else {
      this.clearStoreAndStorage();
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.LiveFeed {
  display: flex;
  flex-direction: column;
  height: 75%;
  width: 60%;
  border: 3px solid #42b983;
  border-radius: 5px;
  overflow-y: scroll;
  scrollbar-color: #2c3e50 #42b983;
  scrollbar-width: thin;
  &__post {
    display: flex;
    flex-direction: column;
    width: 99%;
    border: 1px solid #2c3e50;
    margin: 4px;
    padding-bottom: 7px;
    align-items: center;
    h2 {
      font-size: 20px;
    }
    .likeBtn {
      background-color: #008000;
    }
    fieldset {
      display: flex;
      flex-direction: row;
      justify-content: space-around;
      align-items: center;
      width: 94%;
      height: 60px;
      color: #42b983;
      textarea {
        display: flex;
        max-width: 99%;
        width: 85%;
        height: 70%;
        overflow-y: scroll;
        scrollbar-color: #2c3e50 #42b983;
        scrollbar-width: thin;
      }
    }
    .comments{
      margin-top: 5px;
      // border: 1px solid #2c3e50;
      width: 95%;
      &__list{
        margin:4px;
        border: 1px solid #42b983;
        border-radius: 10px;
      }
    }
  }
}

@media screen and (max-width: 940px) {
  .LiveFeed {
    width: 90%;
  }
}
</style>
