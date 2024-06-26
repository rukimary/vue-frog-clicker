<template>
  <div id="app">
    <nav class="navbar">
      <div v-if="user">
        <p class="navbar_username" @click="logout">{{ user.name }}</p>
      </div>
      <div v-else>
        <button class="navbar_login" @click="toggleModal">Login</button>
      </div>
      <div class="navbar_counter">
        <div class="count">{{ counter }}</div>
        <img
          :src="require(`../assets/fly.gif`)"
          alt="fly_count"
          draggable="false"
        />
      </div>
    </nav>
    <img
      class="main_image"
      :src="currentImage"
      alt="Clickable Image"
      draggable="false"
      @mousedown="changeImage"
      @mouseup="resetImage"
      @mouseleave="resetImage"
      @touchstart="changeImage"
      @touchend="resetImage"
    />

    <LoginModal
      v-if="showLogin"
      :show="showLogin"
      @close="toggleModal"
      @login="handleLogin"
      @toggleModal="toggleModal"
      @setUser="setUser"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import LoginModal from "./LoginModal.vue";
import { getCookie, deleteCookie } from "../services/cookie";

const counter = ref(0);
const image1 = require("../assets/1fr_frog.svg");
const image2 = require("../assets/2fr_frog.svg");
const currentImage = ref(require("../assets/1fr_frog.svg"));
const showLogin = ref(false);
const user = ref(null);
const saveTimeout = ref(null);
const userId = ref(null);

const changeImage = () => {
  currentImage.value = image2;
  incrementCounter();
};
const resetImage = () => {
  currentImage.value = image1;
  saveByTimeout();
};
const saveByTimeout = () => {
  if (saveTimeout.value != null) {
    clearTimeout(saveTimeout.value);
    saveTimeout.value = null;
  }
  saveTimeout.value = setTimeout(() => {
    saveClicks();
    saveTimeout.value = null;
  }, 5000);
};
const incrementCounter = () => {
  counter.value++;
};
const handleLogin = (user) => {
  user.value = user;
};
const logout = () => {
  user.value = null;
  deleteCookie("userId");
  counter.value = 0;
};
const toggleModal = () => {
  showLogin.value = !showLogin.value;
};
const saveClicks = async () => {
  try {
    if (userId.value) {
      const result = await fetch("http://localhost:3000/user/updateClicks", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          id: userId.value,
          clicks: counter.value,
        }),
      });
      const res = await result.json();
      console.log(res.clicks);
    }
  } catch (error) {
    console.log("Error: ", error.message);
  }
};

const getClicks = async () => {
  try {
    if (userId.value) {
      const result = await fetch("http://localhost:3000/user/getMe", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          id: userId.value,
        }),
      });
      const res = await result.json();
      if (res) user.value = res;
      if (res.clicks) counter.value = res.clicks;
      else counter.value = 0;
    }
  } catch (error) {
    console.log("Error: ", error.message);
  }
};

const setUser = () => {
  userId.value = getCookie("userId");
  if (userId.value) {
    getClicks();
  }
};

onMounted(() => {
  setUser();
});
</script>

<style scoped>
.navbar {
  display: flex;
  justify-content: space-between;
  margin: 60px 80px 0 80px;
  min-height: 40px;
}
.navbar_counter {
  display: flex;
  align-items: center;
  gap: 10px;
}
.count {
  text-align: right;
}
.main_image {
  cursor: pointer;
  display: block;
  margin: 30px auto 0;
}
.navbar_username {
  cursor: pointer;
}
.navbar_login {
  cursor: pointer;
  color: white;
  padding: 2px 14px;
  border: solid white 2px;
}
.navbar_login:hover {
  background-color: #5f5f5f;
}
</style>
