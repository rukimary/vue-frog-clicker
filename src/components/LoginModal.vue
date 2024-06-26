<template>
  <div class="modal">
    <div class="modal-content" v-if="showType">
      <span class="close" @click="closeModal">&times;</span>
      <h2 class="title">Login</h2>
      <form class="auth_input" @submit.prevent="login">
        <input
          type="email"
          v-model="loginData.email"
          placeholder="Email"
          required
        />
        <input
          type="password"
          v-model="loginData.password"
          placeholder="Password"
          required
        />
        <button @click="loginUser" class="input_submit" type="submit">
          Login
        </button>
      </form>
      <p class="auth_redirect" @click="loginToggler">Don't have an account?</p>
    </div>

    <div class="modal-content" v-else>
      <span class="close" @click="closeModal">&times;</span>
      <h2 class="title">Register</h2>
      <form class="auth_input" @submit.prevent="login">
        <input
          type="email"
          v-model="registerData.email"
          placeholder="Email"
          required
        />
        <input
          type="text"
          v-model="registerData.name"
          placeholder="Username"
          required
        />
        <input
          type="password"
          v-model="registerData.password"
          placeholder="Password"
          required
        />
        <button class="input_submit" type="submit" @click="registerUser">
          Register
        </button>
      </form>
      <p class="auth_redirect" @click="loginToggler">
        Already have an account?
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, defineEmits } from "vue";
import { setCookie } from "../services/cookie";

const emit = defineEmits(["toggleModal", "setUser"]);

const showType = ref(true);

const registerData = {
  name: "",
  password: "",
  email: "",
};

const loginData = {
  email: "",
  password: "",
};

const loginUser = async () => {
  try {
    const result = await fetch("http://localhost:3000/auth/login", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(loginData),
    });
    const res = await result.json();
    if (res) {
      setCookie("userId", `${res.id}`, { secure: true, "max-age": 86400 });
      // document.cookie = `userId=${res.id}; max-age=86400`;
    }
    emit("setUser");
    emit("toggleModal");
    console.log("Добро " + res.name);
  } catch (error) {
    console.log("Error: ", error.message);
  }
};

const registerUser = async () => {
  try {
    const result = await fetch("http://localhost:3000/auth/register", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(registerData),
    });
    console.log("Зареган!", result);
    emit("toggleModal");
  } catch (error) {
    console.log("Error: ", error.message);
  }
};

const loginToggler = () => {
  showType.value = !showType.value;
};

const closeModal = () => {
  emit("toggleModal");
};
</script>

<style scoped>
.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}
.modal-content {
  padding: 20px;
  display: flex;
  flex-direction: column;
  color: white;
}
.title {
  text-align: center;
  padding-bottom: 40px;
}
.auth_input {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.auth_input input {
  color: white;
}
.input_submit {
  cursor: pointer;
  color: white;
  padding: 2px 14px;
  border: solid white 2px;
}
.input_submit:hover {
  background-color: #5f5f5f;
}
.close {
  cursor: pointer;
  float: right;
}
.auth_redirect {
  cursor: pointer;
}
.auth_redirect:hover {
  cursor: pointer;
  color: #9b9b9b;
}
</style>
