<template>
  <div class="actu" @post-created="refresh">
    <header class="topbar">
      <a href="#" class="topbar-logo"><img alt="Groupomania logo"
          src="../assets/icon-left-font-monochrome-white.svg" /></a>
      <nav class="topbar-nav">
        <form @submit.prevent="handleUnConect" action="#">
          <a href="/actu"><i class="fa fa-home" aria-hidden="true"></i>
            Accueil</a>
          <a href="/profil"><i class="fa fa-user" aria-hidden="true"></i>
            Profil</a>
          <button>Deconnexion</button>
        </form>
      </nav>
    </header>
    <div class="container">
      <h1>Actualité</h1>
      <p>Voici les derniers posts</p>
      <Home />
      <PostCreate />
      <PostList />
      <Footer />
    </div>
  </div>
</template>
<script>
// @ is an alias to /src
// import axios from "axios";
import Home from "@/components/Home.vue";
import PostList from "@/components/PostList.vue";
import PostCreate from "@/components/PostCreate.vue";
import Footer from "@/components/Footer.vue";

export default {
  name: "ActuView",
  components: {
    Home,
    PostList,
    PostCreate,
    Footer
},

  methods: {
    handleUnConect() {
      localStorage.removeItem("userId");
      localStorage.removeItem("token");
      this.$router.push("/login");
    },
  },
};
</script>

<style lang="scss" scoped>
// HEADER
.topbar {
  background: #4E5166;
  color: #fff;
  padding: 16px 0;
  text-align: center;
}
.topbar-logo {
  img {
    margin: 15px;
    padding-left: 16px;
    width: 40%;
  }
}
.topbar a {
  text-decoration: none;
  color: inherit;
}
.topbar-nav {
  padding-bottom: 16px;
}
.topbar-nav a {
  display: inline-block;
  margin: 0 12px;
  color: rgba(255, 255, 255, 0.7);
  transition: color 0.3s;
}
.topbar-nav a:hover,
.topbar-nav a:active {
  color: #fff;
}
button {
  border-radius: 3px;
  border: 1px solid #fd2d01;
  background-color: #fd2d01;
  color: #fff;
  font-weight: bold;
  padding: 15px 5px;
  margin: 10px;
  letter-spacing: 1px;
  text-transform: uppercase;
  cursor: pointer;
  transition: transform 0.1s ease-in;
  &:active {
    transform: scale(0.9);
  }
  &:focus {
    outline: none;
  }
}
// CONTAINER
.container {
  padding: 0 15px;
  max-width: 1050px;
  margin: 0 auto;
}
h1, p {
  display:flex;
  justify-content: center;
}
p{
font-size: 35px;
}

@media only screen and (min-width: 925px) {
  .topbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .topbar-logo {
    margin: 0;
    text-align: left;
  }
  .topbar-logo,
  .topbar-nav {
    padding: 0;
  }
}

@media only screen and (max-width: 925px) {
h1,
  p, h3 {
    font-size: 20px;
  }
}
</style>
