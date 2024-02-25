<template>
  <h2>登入</h2>
  <div class="container">
    <div class="row justify-content-center">
      <h1 class="h3 mb-3 font-weight-normal">請先登入</h1>
      <div class="col-8">
        <form id="form" class="form-signin" @submit.prevent="login">
          <div class="form-floating mb-3">
            <input
              type="email"
              class="form-control"
              id="username"
              placeholder="name@example.com"
              required
              autofocus
              v-model="user.username"
            />
            <!-- v-model綁定 -->
            <label for="username">Email address</label>
          </div>
          <div class="form-floating">
            <input
              type="password"
              class="form-control"
              id="password"
              placeholder="Password"
              required
              v-model="user.password"
            />
            <!-- v-model綁定 -->
            <label for="password">Password</label>
          </div>
          <button
            class="btn btn-lg btn-primary w-100 mt-3"
            type="submit"
            @click="login"
          >
            登入
          </button>
        </form>
      </div>
    </div>
    <p class="mt-5 mb-3 text-muted">&copy; 2021~∞ - 六角學院</p>
  </div>
</template>

<script>
import axios from 'axios';

const { VITE_URL } = import.meta.env;

export default {
  data() {
    return {
      user: {
        // v-model綁定
        username: '', // API資料格式:屬性
        password: '',
      },
    };
  },
  methods: {
    login() {
      const apiUrl = `${VITE_URL}/admin/signin`;
      axios
        .post(apiUrl, this.user)
        // post帶上this.user資料(帳號密碼)
        // 成功登入
        .then((res) => {
          const { token, expired } = res.data;
          // 解構token,expired   token登入憑證  expired時間戳記
          document.cookie = `hexToken=${token}; expires=${new Date(expired)}`;
          this.$router.push('/admin/products');
        })
        .catch(() => {
          // 失敗alert跳出失敗訊息
          alert('登入失敗');
        });
    },
  },
  mounted() {},
};
</script>
