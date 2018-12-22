<template>
  <div class="box">
    <editForm componentName='Products'>
      <div class="box-header with-border">
        <h3 class="box-title">Edit your Profile</h3>
      </div>
      <el-form ref="form" v-if="form" :model="form" label-width="120px">
        <h3>Basic</h3>
        <el-form-item label="name">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
         <el-form-item label="avatar">
        <el-upload
          class="avatar-uploader"
          action=""
          :http-request="postImage"
          name="avatar"
          :show-file-list="false"
          :on-success="handleAvatarSuccess">
          <img v-if="imageUrl" :src="imageUrl" class="avatar">
          <i v-else class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload>
        </el-form-item>
        <el-form-item label="phone">
          <el-input v-model="form.phone"></el-input>
        </el-form-item>
        <el-form-item label="password">
          <el-input type="password" v-model="form.password"></el-input>
        </el-form-item>
        <el-form-item label="fbUrl">
          <el-input v-model="form.fbUrl"></el-input>
        </el-form-item>
        <el-form-item label="description">
          <el-input v-model="form.description"></el-input>
        </el-form-item>
        <el-form-item label="address">
          <el-input v-model="form.address"></el-input>
        </el-form-item>
        <h5>Banking Detail</h5>
        <el-form-item label="Bank Number">
          <el-input v-model="form.bankacc"></el-input>
        </el-form-item>
        <el-form-item label="Bank">
          <el-input v-model="form.bank"></el-input>
        </el-form-item>
      </el-form>
      <div style="float: right">
        <el-button type="success" @click="onSubmit">Edit</el-button>
        <el-button>Cancel</el-button>
      </div>
    </editForm>
  </div>
</template>
<script>
import Edit from '../../partials/Edit.vue'

export default {
  name: 'EditProfile',
  components: {
    'editForm': Edit
  },
  data () {
    return {
      type: [],
      form: {
        name: '',
        phone: '',
        avatar: '',
        address: '',
        description: '',
        password: '',
        bankacc: '',
        bank: ''
      },
      options: [],
      imageUrl: ''
    }
  },
  created () {
    this.$http.get(`api/users/me`).then(res => {
      for (var key in this.form) {
        this.form[key] = res.body[key]
      }
      this.imageUrl = this.$options.filters.takeImage(this.form.avatar)
    })
  },
  methods: {
    onSubmit () {
      this.$http.post(`api/users/me`, this.form).then(res => {
        if (res.status === 200) {
          this.$router.push({path: '/profile'})
        }
      })
    },
    postImage (req) {
      const formData = new FormData()
      formData.append('avatar', req.file)
      this.$http.post('api/pictures/avatar', formData, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      })
        .then(response => {
          console.log(response)
          this.imageUrl = this.$options.filters.takeImage(response.body.avatar)
        })
    },
    handleAvatarSuccess (res, file) {
      this.imageUrl = URL.createObjectURL(file.raw)
    }
  }
}
</script>
<style lang="scss">
  .avatar-uploader .el-upload {
    border: 1px dashed grey;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #20a0ff;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px !important;
    text-align: center;
  }
  .avatar {
    width: 178px;
    height: 178px;
    display: block;
    object-fit: cover;
  }
</style>