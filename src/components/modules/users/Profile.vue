<template>
<div v-if="user">
    <section class="content-header">
        <h1>Profile Page<small>It's all start from here <i class="ti-heart"></i><i class="ti-export"></i><i class="ti-printer"></i></small></h1>
        <ol class="breadcrumb">
            <li>
                <router-link to="/"> <i class="ti-home"></i></router-link>
            </li>
            <li><a href="#">Apps</a></li>
            <li class="active">Profile</li>
        </ol>
    </section>
    <!-- Main content -->
    <section class="content">
        <div class="box">
            <div class="box-body" style="min-height:400px;">
                <div class="movie-card">
                    <div class="container-movie">
                        <a href="#"><img :src="user.avatar | takeImage" alt="cover" class="cover-profile" /></a>
                        <div class="hero">
                            <div class="details-profile">
                                <div class="title1">{{user.name}} <span>Web Designer</span></div>
                                <div class="title2">The Battle of the Five Armies</div>
                            </div>
                        
                            <!-- end details -->
                        </div>
                        <!-- end hero -->
                        <div class="description-profile">
                            <div class="column1 text-center"> <span class="tag">html5</span> <span class="tag">php</span> <span class="tag">javascript</span><span class="tag">css</span><span class="tag">photoshop</span> </div>
                            <!-- end column1 -->
                            <div class="column2">
                                <p>{{user.description}}</p>
                                <div class="row">
                                    <div class="col-xs-12 col-sm-4 emphasis">
                                        <h2>
                                <strong>20,7K</strong>
                            </h2>
                                        <p> <small>Shows</small> </p>
                                        <button class="btn btn-success "> <span class="fa fa-plus-circle"></span>&nbsp;&nbsp;Shows</button>
                                    </div>
                                    <div class="col-xs-12 col-sm-4 emphasis">
                                        <h2>
                                <strong>245</strong>
                            </h2>
                                        <p> <small>Reviews</small> </p>
                                        <router-link :to="{name: 'EditProfile'}" class="btn btn-info "> <span class="fa fa-user"></span>&nbsp;&nbsp;Edit Profile</router-link>
                                    </div>
                                    <div class="col-sm-4 emphasis">
                                        <h2>
                                <strong>43</strong>
                            </h2>
                                        <p> <small>Completed Tasks</small> </p>
                                        <button class="btn btn-default"> <span class="fa fa-user"></span>&nbsp;&nbsp;Income</button>
                                    </div>
                                </div>
                            </div>
                            <!-- end column2 -->
                        </div>
                        <!-- end description -->
                    </div>
                    <!-- end container -->
                </div>
                <!-- end movie-card -->
                <div class="col-sm-12">
                    <br>
                    <br>
                    <div class="col-xs-12 col-sm-12 profile-name">
                        <h2>{{user.name}}
                                <span class="pull-right social-profile">
                                    <i class="entypo-facebook-circled"></i>  <i class="entypo-twitter-circled"></i>  <i class="entypo-linkedin-circled"></i>  <i class="entypo-github-circled"></i>  <i class="entypo-gplus-circled"></i>
                                </span>
                            </h2>
                        <hr>
                        <dl class="dl-horizontal-profile"> 
                            <dt>Email</dt><dd>{{user.email}}</dd> 
                            <dt>Phone</dt><dd>{{user.phone}}</dd> 
                            <dt>Address</dt><dd>{{user.address}}</dd> 
                            <dt>Rating</dt>
                            <dd>
                              <span class="ratings">
                                <a href="#"> <span class="fa fa-star"></span> </a>
                                <a href="#"> <span class="fa fa-star"></span> </a>
                                <a href="#"> <span class="fa fa-star"></span> </a>
                                <a href="#"> <span class="fa fa-star"></span> </a>
                                <a href="#"> <span class="fa fa-star-o"></span> </a>
                              </span>
                            </dd> 
                            <dt>Hobbies</dt><dd>Read, out with friends, listen to music, draw and learn new things</dd> 
                            <!-- <dt>Skills</dt><dd> <span class="tags">html5</span> <span class="tags">css3</span> <span class="tags">jquery</span> <span class="tags">bootstrap3</span> </dd> -->
                        </dl>
                    </div>
                </div>
            </div>
        </div>
    </section>
    </div>
</template>
<script>
  export default {
    name: 'Profile',
    data () {
      return {
        user: null
      }
    },
    created () {
      this.$http.get('api/users/me').then(res => {
        console.log(12, res.body)
        this.user = res.body
      })
    }
  }
</script>
<style scoped>
.cover-profile {
    border-radius: 50%;
    width: 200px;
    height: 200px;
    object-fit: cover;
}
</style>