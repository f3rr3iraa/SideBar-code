<template>
  <div class="flex-column-auto sidebar-profile-background-color">
    <div v-if="visible" class="flex-column sidebar-profile-separator">
      <div class="d-flex h-100">
        <div class="col-4">
          <Avatar
            :image="user.avatarb64"
            class="sidebar-profile-image"
            size="xlarge"
            style="margin-left: 16.5%; margin-top: 16%"
          />
        </div>
        <div class="col-8 d-flex align-items-center">
          <div>
            <div>
              <span>{{ user.name }}</span>
            </div>
            <button
              class="d-flex align-items-center"
              style="border: none; background-color: transparent; padding: 0;"
              @click="teste()"
              >
              <h6 style="margin-right: 5%; margin-top: 10%">Sair</h6>
              <i class="fa-solid fa-right-to-bracket fs-6"></i>
            </button>
          </div>
        </div>
      </div>
    </div>

    <div v-else class="sidebar-profile-separator">
      <div class="app-navbar-item ms-5 ms-md-2 settings-sidebar-profile">
        <div class="sidebar-profile-container-flex">
          <Avatar
            :image="user.avatarb64"
            class="sidebar-profile-image"
            size="xlarge"
            style="margin-left: 2%; margin-top: 30%"
          />
        </div>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: "SideBarProfile",
  props: ["visible", "profileImg"],
  data() {
    return {
      user: [],
    };
  },
  methods: {
    async signOut() {
      await this.logout();
      this.$router.push({ name: "login" });
    },
    teste() {
      console.log("click click");
      this.$socket.emit("disconnectUser");
    },
    getUserData() {
      const userData = localStorage.getItem("user");
      if (userData) {
        this.user = JSON.parse(userData);
        console.log("User data:", this.user);
      } else {
        console.log("User data not found in localStorage.");
      }
    },
  },
  mounted() {
    this.getUserData();
    this.$watch(localStorage.getItem("user"), () => {
      this.getUserData();
    });
    this.$socket.on("confirmDisconnect", () => {
      console.log("Server confirmed disconnection");
      localStorage.removeItem("user");
    });
  },
};
</script>
