<template>
  <div class="form-signin w-100 m-auto">
    <h1 class="h3 mb-3 fw-normal">로그인</h1>

    <div class="form-floating">
      <input type="email" class="form-control" id="floatingInput" placeholder="name@example.com"
             @keyup.enter="submit()" v-model="state.form.email">
      <label for="floatingInput">이메일 주소</label>
    </div>
    <div class="form-floating">
      <input type="password" class="form-control" id="floatingPassword" placeholder="Password" @keyup.enter="submit()"
             v-model="state.form.password">
      <label for="floatingPassword">비밀 번호</label>
    </div>

    <div class="form-check text-start my-3">
      <input class="form-check-input" type="checkbox" value="remember-me" @keyup.enter="submit()" id="flexCheckDefault">
      <label class="form-check-label" for="flexCheckDefault">
        로그인 정보 기억하기
      </label>
    </div>
    <button class="btn btn-primary w-100 py-2" @click="submit()">로그인</button>
    <p class="mt-5 mb-3 text-body-secondary">&copy; 2017–2024</p>

  </div>
</template>

<script>
import {reactive} from "vue";
import axios from "axios";
import store from "@/scripts/store";
import router from "@/scripts/router";

export default {
  setup() {
    const state = reactive({
      form: {
        email: "",
        password: ""
      }
    })

    const submit = () => {
      axios.post("/api/account/login", state.form).then((res) => {
        store.commit('setAccount', res.data);
        console.log(res.data);
        sessionStorage.setItem("id", res.data);
        router.push({path: "/"});
        window.alert("로그인 하였습니다.");
      }).catch(() => {
        window.alert("로그인 정보가 존재하지 않습니다.");
      });
    }

    return {state, submit}
  }
}
</script>
<style scoped>
.form-signin {
  max-width: 330px;
  padding: 1rem;
}

.form-signin .form-floating:focus-within {
  z-index: 2;
}

.form-signin input[type="email"] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}

.form-signin input[type="password"] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
</style>