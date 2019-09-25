<template>
  <div>
    <Navbar></Navbar>
    <Alert></Alert>
    <div style="background-color:rgb(43, 42, 65);padding-top:40px;padding-bottom:1px">
      <router-view></router-view>
    </div>
    <footer style="background-color:rgb(43, 42, 65);" class>
      <div style="display:flex" class="justify-content-center">
        <img src="./img/logo_steam_footer.png" alt />
      </div>
    </footer>
  </div>
</template>

<script>
import Navbar from "./Navbar";
import Alert from "./AlertMessage";

export default {
  components: {
    Navbar,
    Alert
  }
};
</script>

<style scoped>
footer {
  padding-top: 400px;
  padding-bottom: 30px;
}

@media (max-width: 576px) {
  footer {
    padding-top: 30px;
    padding-bottom: 30px;
  }
}
</style>
