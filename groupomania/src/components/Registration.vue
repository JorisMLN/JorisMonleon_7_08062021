<template>
  <div class="Reg">
    <div class="Reg__login">
      <h1>{{ msg }}</h1>
      <div class="formulaire">
        <fieldset>
          <legend>Email *</legend>
          <input type="email" value="test@gmail.com" v-model="form.email" required/>
          <br />
        </fieldset>
        <fieldset>
          <legend>Password *</legend>
          <input type="password" v-model="form.password" required/>
          <br />
        </fieldset>
        <fieldset>
          <legend>Last Name *</legend>
          <input type="text" v-model="form.lastname" required/>
          <br />
        </fieldset>
        <fieldset>
          <legend>First Name *</legend>
          <input type="text" v-model="form.firstname" required/>
          <br />
        </fieldset>
        <fieldset>
          <legend>Job *</legend>
          <input type="text" v-model="form.job" required/>
          <br />
        </fieldset>
        <fieldset>
          <legend>Web Site</legend>
          <input type="text" v-model="form.website" />
          <br />
        </fieldset>
        <fieldset>
          <legend>Hobbies</legend>
          <input type="text" v-model="form.hobbies" />
          <br />
        </fieldset>
        <button v-on:click="submit()"> Confirm </button>
      </div>
      <p> * Required </p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import urlVariable from "@/services/urlVariable.js";

export default {
  name: "Registration",
  props: {
    msg: String,
  },

  data() {
    return {
      urlAPI: urlVariable.getUrl(),
      form: {
        email: "",
        password: "",
        lastname: "",
        firstname: "",
        job: "",
        website: "",
        hobbies: "",
        picture: "http://localhost:3000/images/PhotoProfil.jpg" 
      },
    };
  },

  methods: {
    submit() {
      console.log(this.form);
      if (this.form.email &&
      this.form.password &&
      this.form.lastname &&
      this.form.firstname &&
      this.form.job){
        axios
        .post(this.urlAPI + "api/user/signup", this.form)
        .then((response) => {
          console.log(response.data);
          window.location = "http://localhost:8080/#/";
        })
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
.Reg {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  height: 65vh;
  &__login {
    display: flex;
    justify-content: space-around;
    flex-direction: column;
    align-items: center;
    border: 1px solid green;
    border-radius: 5px;
    width: 400px;
    height: 100%;
    h1{
      height: 10%;
    }
    .formulaire {
      height: 90%;
      width: 80%;
      display: flex;
      align-items: center;
      justify-content: space-around;
      flex-direction: column;
      fieldset{
        width: 90%;
        height: 7%;
        color: #42b983;
        input{
          width: 70%;
        }
      }
      button {
        margin-top: 7px;
        margin-bottom: 7px;
        height: 8%;
        width: 50%;
      }
    }
    p {
      font-size: 15px;
      width: 100px;
      margin: 10px;
      padding: 2px;
      color: #2c3e50;
    }
  }
}
</style>
