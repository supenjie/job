<template>
  <div class="box">
    <el-tabs v-model="activeName">
      <el-tab-pane label="用户管理" name="first">控制台</el-tab-pane>
      <el-tab-pane label="配置管理" name="second"><JobTwo /></el-tab-pane>
    </el-tabs>
  </div>
</template>
<script>
import axios from "axios";
import md5 from "js-md5";
import JobTwo from "./JobTwo.vue";
const url = "https://www.zzgoodqc.cn/index.php/index/callcenter/getheaders";
const strheader = "header";
const name = "name";
const role = "role";
const str = "nbsp123ok";
const sign = md5(name + role + str + strheader);
axios
  .post(
    url,
    { name, role, sign },
    {
      headers: {
        strheader: strheader,
      },
    }
  )
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error(error);
  });
export default {
  data() {
    return {
      activeName: "second",
    };
  },
  components: {
    JobTwo,
  },
  methods: {},
};
</script>
<style scoped>
.box {
  width: 1200px;
  height: 90vh;
  margin: auto;
}
</style>
