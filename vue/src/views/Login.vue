<template>
  <div id="login">
    <RandomCatPic mission="Welcome Back Please Sign In" />
    <form @submit.prevent="login">
      <div role="alert" v-if="invalidCredentials">
        Invalid username and password!
      </div>
      <div role="alert" v-if="this.$route.query.registration">
        Thank you for registering, please sign in.
      </div>
      <div class="form-input-group">
        <label for="username">Username</label>
        <input
          type="text"
          id="username"
          v-model="user.username"
          required
          autofocus
        />
        <label for="password">Password</label>
        <input type="password" id="password" v-model="user.password" required />
        <button type="submit">Sign in</button>
        <p>
          <router-link :to="{ name: 'volunteer' }"
            >Need an account? Sign up.</router-link
          >
        </p>
      </div>
    </form>
    <NavBar />
    <!-- <PetCarousel /> -->
    <Footer />
  </div>
</template>

<script>
import Footer from "../components/Footer.vue";
import NavBar from "../components/NavBar.vue";
// import PetCarousel from "../components/PetCarousel.vue";
import authService from "../services/AuthService";
import RandomCatPic from "../components/RandomCatPic.vue";

export default {
  name: "login",
  components: {
    NavBar,
    RandomCatPic,
    // PetCarousel,
    Footer,
  },
  data() {
    return {
      user: {
        username: "",
        password: "",
      },
      invalidCredentials: false,
    };
  },
  methods: {
    login() {
      authService
        .login(this.user)
        .then((response) => {
          if (response.status == 200) {
            this.$store.commit("SET_AUTH_TOKEN", response.data.token);
            this.$store.commit("SET_USER", response.data.user);
            this.$router.push("/");
          }
        })
        .catch((error) => {
          const response = error.response;

          if (response.status === 401) {
            this.invalidCredentials = true;
          }
        });
    },
    methods: {},
    created() {},
  },
};
</script>

<style scoped>
.form-input-group {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  height: 50vh;
  background: #a28f9d;
  font-family: "sans-serif";
  font-size: 20px;
}

label {
  margin-right: 0.5rem;
}
</style>