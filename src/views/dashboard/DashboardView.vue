<template>
  <h2>這是後台</h2>
  <nav>
    <RouterLink to="/admin/products">產品列表</RouterLink> |
    <RouterLink to="/admin/order">訂單列表</RouterLink> |
    <RouterLink to="/">回到前台</RouterLink> |
  </nav>
  <RouterView> </RouterView>
</template>

<script>
import axios from 'axios';

const { VITE_URL } = import.meta.env;

export default {
  data() {
    return {
      apiUrl: 'https://vue3-course-api.hexschool.io/v2',
      apiPath: 'orzorzorz',
      products: [], //  定義資料格式
      tempProduct: {}, //   將products 放到tempProduct 暫存區 用於存取查看單一產品資料
    };
  },

  methods: {
    checkAdmin() {
      //    驗證
      const url = `${VITE_URL}/api/user/check`;
      axios
        .post(url)
        .then((res) => {
          console.log('驗證成功:', res.data.success);
        })
        .catch((err) => {
          console.log(err);
          this.$router.push('/login');
        });
    },
  },
  mounted() {
    // 掛載
    // 取得 Token
    const token = document.cookie.replace(
      /(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/,
      '$1',
    );
    axios.defaults.headers.common.Authorization = token;
    // 將token內容加到 headers裡
    // https://github.com/axios/axios?tab=readme-ov-file#-axiosheaders
    this.checkAdmin();
  },
};
</script>
