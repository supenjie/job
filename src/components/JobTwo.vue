<template>
  <div id="app">
    <el-form ref="form" :model="form" label-width="80px">
      <el-form-item label="姓名">
        <el-input v-model="form.name"></el-input>
      </el-form-item>
      <div class="img">
        <span>头像</span>
        <el-upload
          class="avatar-uploader"
          action="https://47.94.4.201/index.php/index/upload/uploadimg"
          :show-file-list="false"
          :on-success="handleAvatarSuccess"
        >
          <img v-if="imageUrl" :src="imageUrl" class="avatar" />
          <i v-else class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload>
      </div>
      <el-form-item label="昵称">
        <el-input v-model="form.nickname"></el-input>
      </el-form-item>
      <el-form-item label="性别">
        <el-radio-group v-model="form.resource">
          <el-radio label="男"></el-radio>
          <el-radio label="女"></el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="出生日期">
        <el-col :span="11">
          <el-date-picker
            type="date"
            placeholder="选择日期"
            v-model="form.date1"
            style="width: 217.5%"
          ></el-date-picker>
        </el-col>
      </el-form-item>
      <div class="block">
        <span class="demonstration">&emsp;&emsp;籍贯&emsp;</span>
        <el-cascader
          v-model="value"
          :options="options"
          style="width: 93.2%"
        ></el-cascader>
      </div>
      <el-form-item label="手机号">
        <el-input
          v-model="form.number"
          v-only-numbers
          maxlength="11"
        ></el-input>
      </el-form-item>
      <el-form-item label="邮箱">
        <el-input v-model="form.email"></el-input>
      </el-form-item>

      <el-form-item label="个人介绍">
        <el-input type="textarea" v-model="form.desc"></el-input>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="onSubmit">提交</el-button>
        <el-button>取消</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
Vue.directive("onlyNumbers", {
  bind: function (el) {
    el.addEventListener("input", function (e) {
      if (e.target.value.length > 0) {
        e.target.value = e.target.value.replace(/[^\d]/g, "");
      }
    });
  },
});
export default {
  data() {
    return {
      form: {
        name: "", //姓名
        nickname: "", //昵称
        number: "", //手机号
        email: "", //邮箱
        date1: "", //出生日期
        type: [], //出生日期
        resource: "", //性别
        desc: "", //个人介绍
        id: 9999, //id
      },
      imageUrl: "", //图片
      value: [],

      //   籍贯
      options: [
        {
          value: "zhinan",
          label: "指南",
          children: [
            {
              value: "shejiyuanze",
              label: "设计原则",
              children: [
                {
                  value: "yizhi",
                  label: "一致",
                },
                {
                  value: "fankui",
                  label: "反馈",
                },
                {
                  value: "xiaolv",
                  label: "效率",
                },
                {
                  value: "kekong",
                  label: "可控",
                },
              ],
            },
            {
              value: "daohang",
              label: "导航",
              children: [
                {
                  value: "cexiangdaohang",
                  label: "侧向导航",
                },
                {
                  value: "dingbudaohang",
                  label: "顶部导航",
                },
              ],
            },
          ],
        },
      ],
    };
  },
  methods: {
    // 提交
    onSubmit() {
      const apiUrl = "http://47.94.4.201/index.php/index/index/mycenter";
      const params = {
        name: this.form.name,
        nick: this.form.nickname,
        userid: this.form.id++,
        iphone: this.form.number,
        // email: this.form.email,
        // date1: this.form.date1,
        // sex: this.form.resource,
        // desc: this.form.desc,
        img: this.imageUrl,
      };
      console.log(params);
      axios
        .post(apiUrl, params)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          console.error(error);
        });
    },

    handleAvatarSuccess(res, file) {
      const reader = new FileReader();
      reader.onload = (event) => {
        const base64Data = event.target.result;
        this.uploadImage(base64Data);
      };
      reader.readAsDataURL(file.raw);
    },
    uploadImage(base64Data) {
      const apiUrl = "http://47.94.4.201/index.php/index/upload/uploadimg";
      const params = {
        imgurl: base64Data,
      };
      axios
        .post(apiUrl, params)
        .then((response) => {
          const imageUrl = response.data.data.url;
          this.imageUrl = imageUrl;
          console.log(response, "http://47.94.4.201/" + imageUrl);
          // 将imageUrl赋值给data的某个属性
          this.imageUrl = "http://47.94.4.201/" + imageUrl;
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  list-style: none;
}
.block {
  margin: 20px 0;
}
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
.img {
  width: 500px;
  height: 200px;
  /* background-color: red; */
  margin-bottom: 20px;
  margin-left: 37px;
}
.avatar-uploader {
  margin-left: 100px;
  border: 1px solid black;
  text-align: center;
}
</style>
