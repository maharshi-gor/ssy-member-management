<template>
  <div id="app">
    <nav id="nav">
      <span>BGBS - SSY - Member management</span>
      <span class="small-text" @click="logOut" v-if="isLoggedIn"> Logout <i class="fas fa-sign-out-alt"></i></span>
    </nav>
    <div id="body" v-if="!isLoggedIn">
      <Login></Login>
    </div>
    <div id="body" v-if="isLoggedIn">
      <aside id="sidebar">
        <Sidebar></Sidebar>
      </aside>
      <div id="content">
        <router-view />
      </div>
    </div>
    <Loading v-if="showLoader"></Loading>
  </div>
</template>

<script>
import { fa } from './firebase/firebaseinit'
import { mapGetters } from 'vuex'
import Loading from './components/Loading'
import Sidebar from './components/Sidebar'
import Login from './components/Login'

export default {
  components: {
    Loading,
    Sidebar,
    Login
  },
  data () {
    return {
      isLoggedIn: true
    }
  },
  computed: {
    ...mapGetters({
      showLoader: 'getShowLoading'
    })
  },
  methods: {
    logOut () {
      fa.signOut()
    }
  },
  created () {
    fa.onAuthStateChanged(firebaseUser => {
      if (firebaseUser) {
        this.isLoggedIn = true
      } else {
        this.isLoggedIn = false
        if (this.$route.path !== '/') {
          this.$router.push('/')
        }
      }
    })
  }
}
</script>

<style lang="scss">
@import "./styles/colors.scss";
@import "./styles/style.scss";

body {
  margin: 0;
  font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif;
  font-size: 0.9rem;
}

#nav {
  position: fixed;
  top: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 1vw;
  height: 6vh;
  width: 98vw;
  box-shadow: 0 3px 2px 1px $color-support;
  background-color: $color-blue;
  color: white;
  font-size: 20px;
}

.small-text {
  font-size: 16px;
  cursor: pointer;
}

#body {
  margin-top: 6vh;
  display: flex;
  height: 94vh;
}

#sidebar {
  width: 15vw;
  background-color: #fff;
}

#content {
  width: 85vw;
  overflow: scroll;
  background-color: $color-background;
}
</style>
