<template>
  <div class="hello">
    <!--<el-upload-->
      <!--class="avatar-uploade"-->
      <!--:multiple="true"-->
      <!--:action="actionPath"-->
      <!--accept="image/jpeg,image/gif,image/png,image/bmp"-->
      <!--:data="postData"-->
      <!--:auto-upload="false"-->
      <!--:before-upload="beforeAvatarUpload"-->
      <!--:on-success="handleAvatarSuccess">-->
      <!--<img v-if="imageUrl" :src="imageUrl" class="avatar">-->
      <!--<el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload">上传到服务器</el-button>-->
      <!--<i  class="el-icon-plus avatar-uploader-icon"></i>-->
    <!--</el-upload>-->

    <el-upload
      class="upload-demo"
      ref="upload"
      :action="actionPath"
      :before-upload="beforeAvatarUpload"
      :on-success="handleAvatarSuccess"
      :file-list="fileList"
      list-type="picture-card"
      :data="postData"
      :limit="3"
      :on-exceed="handleExceed"
      >
      <i class="el-icon-plus padding-plus" @click="getToken"></i>
    </el-upload>


  </div>
</template>

<script>

  import axios from 'axios'
  import uuid from 'uuid'

  export default {
    name: 'HelloWorld',
    data() {
      return {
        msg: 'Welcome to Your Vue.js App',
        actionPath: 'https://up-z1.qiniup.com',
        imageUrl: '',
        postData: {
          token: '',
          key: ''
        },
        fileList: [],
        dialogImageUrl: '',
        dialogVisible: false
      }
    },

    methods: {

      getToken() {
        var key = 'reddot/' + uuid.v4()
        this.postData.key = key
        var param = {
          key: key
        }
        axios.post('/api/getToken', param).then(res => {
          // console.log(res.data.result)
          this.postData.token = res.data.result;
          // return next
        })
      },
      beforeAvatarUpload(file, next)
      {
        // this.getToken()
        console.log(file)
      },
      handleAvatarSuccess(res)
      {
        var url = 'http://wsmpage.cn/'
        console.log(url + res.key)
      },
      handleExceed(files, fileList) {
        this.$notify({
          title: '警告',
          message: '一次反馈最多只能提交3张图片',
          offset: 46
        });
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }

  .padding-plus {
    padding: 60px;
  }

</style>
