<template>
    <div>
        <!-- 上传标题 -->
        <div class="block">{{uploadTitle}}</div>
        <!-- 上传图标 -->
        <el-upload
        class="avatar-uploader"
        action="/api/upload"
        :show-file-list="false"
        :on-success="handleAvatarSuccess"
        :headers="headers"
        >
        <img v-if="value" :src="imageUrl" class="avatar">
        <i v-else class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload>
    </div>
</template>

<script>
    import {server_URL} from "@/urlConfig.js"
    export default {
        props: ['uploadTitle', 'value' ],
        computed: {
            imageUrl() {
                if(this.value) {
                    // return server_URL + this.value
                    return this.value
                }
            },
            headers() {
                return {
                    Authorization: 'Bearer ' + localStorage.getItem('adminToken')
                }
            }
        },
        methods: {
            handleAvatarSuccess(res) {
                console.log('上传',res);
                if(!res.code && res.data) {
                    // 说明上传成功，服务器给我们返回了图片
                    this.$emit('input', res.data);
                } 
                this.$message.success('上传成功')
            },
        },
        
    }
</script>

<style scoped>
  .block{
        margin: 15px 0;
        font-weight: 100;
  }
  .avatar-uploader .el-upload {
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
  }
  .avatar {
    width: 178px;
    height: 178px;
    display: block;
  }

</style>