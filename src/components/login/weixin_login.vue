<template>
    <div class="main">
        <div class="login_img"><img class="xinli" :src="logoUrl" alt=""></div>
        <button class="tel_login" @click="phoneGo()">手机号登录</button>
        <button class="weixin_login" @click="weixinGo()">微信登录</button>
    </div>
</template>
<script>
export default {
    data() {
      return {
        logoUrl:'../../../static/images/l_logo.png'
      };
    },

    //获取logo接口
     created () {
      console.log('created');
        this.axios.get(this.$baseurl + '/api/config/getConfig',
        	{
        		params: {
        			param: 'logoUrl'     		
        		}
        	}
        ) .then( res => {
                   console.log(res);
                   this.logoUrl=res.data.logoUrl
           		
           	}).catch( err => {
           		console.log(err);
           	})
        },
    methods:{
       phoneGo () {
           this.$router.push({path: '/login'})
       },
       weixinGo () {
           //微信登录接口
            console.log('created');
        this.axios.get(this.$baseurl + '/api/config/getConfig',
        	{
        		params: {
        			param: 'banner'     		
        		}
        	}
        ) .then( res => {
                   console.log(res);          		
           	}).catch( err => {
           		console.log(err);
           	})
           this.$router.push({path: '/test'})
       }
    }

};
</script>
<style lang="scss" scoped>
 .main{
        width: 100%;
        background-color: #f8f8f8;
        position: relative;
        .login_img{
            padding-top: 2rem;
            box-sizing: border-box;
            -webkit-box-sizing: border-box;
            text-align: center;
       .xinli{
            display: block;
            width: 2.13rem;
            height: 2.02rem;
            margin: 0 auto;
           }
          
        }
        .tel_login{
            margin: 0 auto;
            width: 5.96rem;
            height: 0.89rem;
            background-color: #ff8043;
            color:#fff;
            font-size: 0.35rem;
            border-radius: 20px;
            position: absolute;
            left: 50%;
            margin-left: -3rem;
            top:5.5rem;
        }
        .weixin_login{
            width: 5.96rem;
            height: 0.89rem;
            background-color: #6cc130;
             color:#fff;
            font-size: 0.35rem;
            border-radius: 20px;
             position: absolute;
            left: 50%;
            margin-left: -3rem;
            top:6.75rem;
        }
 }

</style>


