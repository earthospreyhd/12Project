<template>
  <div>
    <!-- dropdown needs to be outside of navbar because of z-index cooks -->

    <div class="navbar">
      <!-- <a class="brand navlink">
        <h2>Git Money</h2>
      </a> -->
      <img
        width="45px"
        height="45px"
        src="../assets/logo.svg"
        alt="logo"
        class="brand navlink"
      />
      <router-link to="/" class="navlink">Home</router-link>
      <template v-if="!userIsAuth">
        <router-link to="/explore" class="navlink">Explore</router-link>
      </template>
      <router-link to="/projects" class="navlink">Browse Projects</router-link>
      <template v-if="userIsAuth">
        <router-link to="/myprojects" class="navlink">My Projects</router-link>
      </template>
      <div class="rightContent">
        <span class="profile-dropdown" v-if="userIsAuth == true">
          <span class="profile-overflow"
            ><img
              v-bind:src="imgurl"
              alt="Profile Picture"
              class="profile-picture"
              v-if="imgurl"
            />
          </span>
          <ul class="dropdown">
            <li class="dropdown-el button" @click="signout">
              Sign Out
            </li>
            <li
              class="dropdown-el button"
              @click="goToPage(`/myprofile`)"
              style="text-decoration:none; margin-top: 10px;"
            >
              Profile
            </li>
          </ul>
        </span>

        <!-- <el-button
        v-if="userIsAuth === true"
        v-on:click="signOut"
        type="primary"
        plain
        >Sign out</el-button
      > -->
        <button
          v-else-if="userIsAuth === false"
          v-on:click="goToLogin"
          type="primary"
          class="button login-button"
        >
          Login/Register
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import store from "@/Vuex/store";
import { signOut } from "@/firebase/firebase";
export default {
  data() {
    return {
      showDropdown: false
    };
  },
  computed: {
    userIsAuth: () => {
      return store.state.userIsAuth;
    },
    imgurl: () => {
      return store.state.userPictureURL;
    },
    isDrop: () => {
      return this.showDropdown;
    }
  },
  methods: {
    goToPage(url) {
      this.$router.push(url);
    },
    signout() {
      signOut().then(() => {
        this.$router.push("/");
      });
    },
    goToLogin() {
      this.$router.push({ name: "login" });
    }
  }
};
</script>
<style lang="scss" scoped>
.drop-leave-active,
.drop-enter-active {
  transition: opacity 0.4s;
}
.drop-enter,
.drop-leave-to {
  opacity: 0;
}
.dropdown-el {
  list-style: none;
  text-align: center;
  text-decoration: none !important;
}
.dropdown-el:hover {
  cursor: pointer;
}
.dropdown {
  padding: 0;
  margin: 0;
  display: inline-block;
  position: absolute;
  text-align: center;
  top: 71px;
  width: 150px;
  height: 150px;
  right: 0;
  background-color: white;
  border: 1px solid #ebeef5;
  border-bottom-left-radius: 10px;
  transition-duration: 0.2s;
  opacity: 0;
  visibility: hidden;
}
.profile-dropdown:hover .dropdown {
  opacity: 1;
  visibility: visible;
}
.profile-dropdown {
  text-decoration: none;
  font: inherit;
  box-sizing: inherit;
  padding: 10px 15px 10px 15px;
  color: #2c3e50;
  display: flex;
  align-items: center;
  height: 51px;
  float: left;
}
.profile-dropdown:hover {
  cursor: pointer;
}
.profile-overflow {
  border-radius: 50%;
  border: 1px solid #ebeef5;
  height: 47px;
  width: 47px;
  overflow: hidden;
  display: inline-block;
}
.profile-picture {
  max-width: 45px;
  height: 45px;
}
.navlink {
  text-decoration: none;
  padding: 10px 15px 10px 15px;
  color: #2c3e50;
  display: flex;
  align-items: center;
  height: 49px;
  float: left;
}
.brand {
  padding-right: 20px;
  padding-left: 20px;
  margin-top: 2px;
}
.router-link-exact-active {
  border-bottom: 2px solid;
  border-bottom-color: #409eff;
  animation: border-fade-in 0.4s;
}
@keyframes border-fade-in {
  from {
    border-bottom-color: rgba(64, 158, 255, 0);
  }
  to {
    border-bottom-color: rgba(64, 158, 255, 1);
  }
}
.rightContent {
  float: right;
  line-height: 70px;
  padding: 0 10px 0 10px;
}
.navbar {
  position: relative;
  top: 0;
  width: 100%;
  display: inline-block;
  -webkit-box-shadow: 0 1px 7px 0 rgba(0, 0, 0, 0.1);
  box-shadow: 0 1px 7px 0 rgba(0, 0, 0, 0.1);
  z-index: 2;
  background-color: white;
}
.center {
  text-align: center;
}
</style>
