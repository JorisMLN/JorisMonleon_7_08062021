<template>
  <div class="postCreation">
    <fieldset>
      <legend>Partagez avec vos super collègues !</legend>
      <textarea type="text" v-model="form.text" />
      <div class="gif-poster">
        <button class="poster" v-on:click="submit()">Poster</button>
      </div>
    </fieldset>
  </div>
</template>

<script>
import urlVariable from "@/services/urlVariable.js";

export default {
  name: "PostCreation",

  data() {
    return {
      urlAPI: urlVariable.getUrl(),
      form: {
        text: "",
        userId: this.$store.state.userId,
        firstname: this.$store.state.firstname,
        lastname: this.$store.state.lastname,
      },
      token: this.$store.state.token,
    };
  },

  methods: {
    submit() {
      console.log(this.form);
      if (this.form.text){
        const requestOptions = {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            authorization: "Bearer: " + this.token,
          },
          body: JSON.stringify({ post: this.form }),
        };
        fetch(this.urlAPI + "api/posts", requestOptions)
          .then((response) => response.json())
          .then(
            (data) => (this.postId = data.id),
            (window.location = "http://localhost:8080/#/feed")
          )
          .catch((e) => {
            console.log(e);
          });
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.postCreation {
  display: flex;
  justify-content: space-around;
  flex-direction: column;
  align-items: center;
  border-radius: 5px;
  height: 98%;
  width: 99%;
  overflow: hidden;
  fieldset {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 95%;
    height: 85%;
    color: #398466;
    textarea {
      max-width: 99%;
      width: 99%;
      height: 85%;
      overflow-y: scroll;
      scrollbar-color: #2c3e50 #42b983;
      scrollbar-width: thin;
    }
    .poster {
      width: 150px;
    }
  }
}
</style>
