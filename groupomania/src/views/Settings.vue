<template>
  <div class="settings">
    <Picture msg="Change your profile picture ?" />
    <ChangeInfo msg="Change your profile info ? (please fill in all inputs)" />
    <DeleteAccount msg="Delete your groupomania account ?" unsub="delete Account" />
    <Footer
      beforeMsg="© 2020-2021, "
      msg="Groupomania.git, Inc."
      beforeSigned="Created by "
      signed="'Vingilot Studio ©'"
    />
  </div>
</template>

<script>
// @ is an alias to /src
import Picture from "@/components/Picture.vue";
import ChangeInfo from "@/components/ChangeInfo.vue";
import DeleteAccount from "@/components/DeleteAccount.vue";
import Footer from "@/components/Footer.vue";

export default {
  name: "Settings",
  components: {
    Picture,
    ChangeInfo,
    DeleteAccount,
    Footer
  },
};
</script>

<style scoped lang="scss">

.settings {
    height: 88vh;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
}

@media screen and (max-width: 740px) {
  .settings {
    height: 120vh;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
}
}
</style>
