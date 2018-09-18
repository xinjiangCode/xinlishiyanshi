<template>
    <div class="main">
        <img :src="logoUrl" alt="">
        <div class="login_ipt">
            <div class="phone">
                <img src="../../../static/images/l_phone.png" alt="">
                <input class="phone_ipt" type="text" maxlength="11" v-model="phone" placeholder="请输入手机号码">
            </div>
            <div class="pass">
                <img src="../../../static/images/l_pass.png" alt="">
                <input class="pass_ipt" type="password" v-model="password" placeholder="请输入密码">
            </div>
        </div>
        <div class="go_other">
            <span class="go_regist"  @click="phoneRegisteredd()">手机注册</span>
            <i class="go_forgot" @click="phoneRegistered()">忘记密码</i>
        </div>
        <div class="login" @click="login()">手机号登录</div>
        <div class="wx_login" @click="weixin()">微信登录 </div>
        <div class="myMap"></div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            password:'',
            phone:'',
            inputType:'password',
            logoUrl:'../../../static/images/l_logo.png',
        }
    },
    mounted:function () {
    },
    computed:{
    },
    watch:{
        phone:function () {
            this.phone=this.phone.replace(/\D/g,'');
        }
    },
    created () {
        //获取logo接口
      console.log('created');
        this.axios.get(this.$baseurl + '/api/config/getConfig',
        	{
        		params: {
        			param: 'logoUrl'     		
        		}
        	}).then( res => {
                   console.log(res);
                   this.logoUrl=res.data.logoUrl
           		
           	}).catch( err => {
           		console.log(err);
           	})
        },
    methods: {
        weixin:function(){
            this.$router.push({path: '/weixin_login'});
             var token=res.data.token
                 localStorage.setItem('token', token);
        },
        login: function () {
           //手机号等验证
            var that=this;
            if(this.phone == ''){
                this.$Toast({
                    message: '手机号不能为空',
                    position: 'bottom'
                });
                return;
            }
            var phone=/^1[34578]\d{9}$/;
            if(!phone.test(this.phone)){
                this.$Toast({
                    message: '手机号格式错误',
                    position: 'bottom'
                });
                return;
            }
            if(this.password == ''){
                this.$Toast({
                    message: '密码不能为空',
                    position: 'bottom'
                });
                return;
            }
           var password=/^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{6,12}$/;
            if(!password.test(this.password)||password.length<6||password.length>12){
                this.$Toast({
                    message: '密码必须为6-12位的数字和字母的组合',
                    position: 'bottom'
                });
                return;
            }
          //手机号登录接口
           console.log('created');
            this.axios({
            method:"post",
            url:this.$baseurl + "/api/member/login",
            params:{
                phone:this.phone,
                password: this.password,
            }
            }) .then( res => {
            var code=res.data.code
             if(code=="500"){
                  this.$Toast({
                        message: '密码错误',
                        position: 'bottom'
            });
            return;
            }
                this.$router.push({path:'/test'})
                var token=res.data.token;
                console.log(token);
                localStorage.setItem('token', token);
            }).catch( err => {
              console.log(err);
         });
          
        },
        phoneRegistered: function(er) {
            this.$router.push({path:'/password'})
        },
        phoneRegisteredd () {
            this.$router.push({path: '/register'})
        }
    },
 

}
</script>

<style lang="scss" scoped>
    .main{
        width: 100%;
        padding-top: 2rem;
        img{
            display: block;
            width: 2.13rem;
            margin: 0 auto;
            margin-bottom: 1.45rem;
        }
        .login_ipt{
            width: 6rem;
            margin: 0 auto;
            .phone{
                width: 100%;
                height: 0.55rem;
                overflow: hidden;
                border-bottom: 1px solid #cfcfcf;
                img{
                    display: block;
                    width: 0.27rem;
                    float: left;
                    margin: 0.1rem 0.4rem 0 0.33rem;
                }
                input{
                    display: block;
                    width: 5rem;
                    height: 0.55rem;
                    font-size: 0.3rem;
                    color: #333333;
                    float: left;
                    border: 0;
                    outline: none;
                    background: #f8f8f8;
                }
            }
            .pass{
                width: 100%;
                height: 0.55rem;
                overflow: hidden;
                border-bottom: 1px solid #cfcfcf;
                margin-top: 0.65rem;
                margin-bottom: 0.6rem;
                img{
                    display: block;
                    width: 0.27rem;
                    float: left;
                    margin: 0.1rem 0.4rem 0 0.33rem;
                }
                input{
                    display: block;
                    width: 5rem;
                    height: 0.55rem;
                    font-size: 0.3rem;
                    color: #333333;
                    float: left;
                    border: 0;
                    outline: none;
                    background: #f8f8f8;
                }
            }
        }
        .go_other{
            width: 5.6rem;
            height: 0.7rem;
            line-height: 0.7rem;
            margin: 0 auto;
            overflow: hidden;
            font-size: 0.3rem;
            color: #353535;
            span{
                float: left;
            }
            i{
                float: right;
            }
        }

        .login{
            width: 6rem;
            height: 0.9rem;
            background: #ff8042;
            margin: 0 auto;
            font-size: 0.36rem;
            color: white;
            line-height: 0.9rem;
            text-align: center;
            border-radius: 0.45rem;
        }
        .wx_login{
            width: 6rem;
            height: 0.9rem;
            margin: 0 auto;
            background: #6cc130;
            font-size: 0.36rem;
            color: white;
            line-height: 0.9rem;
            text-align: center;
            border-radius: 0.45rem;
            margin-top: 0.38rem;
        }
    }
</style>
