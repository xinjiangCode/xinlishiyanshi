<template>
    <div class="main">
       <div class="header clearfix">
         <a @click="back()"><img src="../../../static/images/h_return.png" alt=""></a>
         {{mgs}}
      </div>
        <div class="login_img"><img class="xinli" :src="logoUrl" alt=""></div>
        <ul class="login_ipt">
            <li class="phone">
                <img src="../../../static/images/l_phone.png" alt="">
                <input class="phone_ipt" type="text" maxlength="11"  v-model="phone" placeholder="请输入手机号码">
            </li>
             <li class="pass">
                <img src="../../../static/images/l_pass.png" alt="">
                <input class="pass_ipt" type="password"  v-model="password" placeholder="请输入密码">
            </li>
            <li class="verify">
                <img src="../../../static/images/verify_03.png" alt="">
                <input class="pass_ipt" type="password"  v-model="authCode" placeholder="请输入验证码">
                <button class="verify_ri" id="yanzhe" @click="gainCode()">获取验证码</button>
                <button v-show="!show" class="count">{{count}} s</button>
            </li>

             <li class="read">
            <img class="read_img" :src="src" alt="" @click="changeImg()">
            <span class="read_le" >已阅读并且同意<span class="read_ri">《用户服务协议》</span></span>
        </li>
            
        </ul>
       
        <button class="immider"  @click="loginGo()" >立即注册 </button>
    </div>
</template>
<script>

export default {
    data() {
      return {
        mgs:'手机注册',
        logoUrl:'../../../static/images/l_logo.png',
        phone:'',
        password:'',
        authCode:'',
        src:'../../../static/images/notread.png',
        show:true,
        count:'',
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
        	}) .then( res => {
                   console.log(res);
                   this.logoUrl=res.data.logoUrl
           		
           	}).catch( err => {
           		console.log(err);
           	})
        },

    watch:{
        authCode:function () {
            this.phone=this.phone.replace(/\D/g,'');
            if(this.phone == ""){
                document.getElementById('yanzhe').style.backgroundColor = none;
            } else {
                document.getElementById('yanzhe').style.backgroundColor = none;
            }
            
        },
    },
    methods:{
        back () {
            history.back();
        },
        //获取验证码接口
        gainCode () {
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

         

             const TIME_COUNT = 60;
                if (!this.timer) {
                this.count = TIME_COUNT;
                this.show = false;
                this.timer = setInterval(() => {
                if (this.count > 0 && this.count <= TIME_COUNT) {
                    this.count--;
                    } else {
                    this.show = true;
                    clearInterval(this.timer);
                    this.timer = null;
                    }
                }, 1000)
                }; 



           console.log('created');
            this.axios({
            method:"post",
            url:this.$baseurl + "/api/register/getMessage",
            params:{
                phone:this.phone,
            }
            }) .then( res => {
            
            var code = res.data.code;
             if(code == "0"){
                this.$Toast({
                    message: '发送验证码成功',
                    position: 'bottom'
                });
            }
            }).catch( err => {
              console.log(err);
         });
        },
        
        loginGo () {  
              if(this.authCode == ''){
                this.$Toast({
                    message: '验证码不能为空',
                    position: 'bottom'
                });
                return;
            }
            
            var img1 = "../../../static/images/read.png"
            if(this.src != img1){
                 this.$Toast({
                    message: '请选中已阅读',
                    position: 'bottom'
                });
                  return false
            }
            //注册接口
              console.log('created');
                this.axios({
                method:"post",
                url:this.$baseurl + "/api/register/register",
                params:{
                    phone:this.phone,
                    password:this.password,
                    codes:this.authCode,
                }
                }) .then( res => {
                    var code = res.data.code;
                    if(code=="1"){
                         this.$Toast({
                        message: '验证码错误',
                        position: 'bottom'
                });
                     return;
             }
                    if(code=="0"){
                        this.$router.push({path:'/login'})
             }       
                }).catch( err => {
                  console.log(err);
             });
          
        },
    changeImg () {
        var img = "../../../static/images/notread.png"
        if(this.src == img){
          this.src = "../../../static/images/read.png"
        }else{
           this.src = "../../../static/images/notread.png"
        }
      }, 
       
    }

};
</script>
<style lang="scss" scoped>
.main {
  width: 100%;
  background-color: #f8f8f8;
  position: relative;
  .header{
        height: 0.88rem;
        line-height: 0.88rem;
        font-size:0.36rem;
        background-color: #2083d1;
        color:#fff;
        text-align: center;
        a{
            display: block;
            width: 0.4rem;
            height: 0.5rem;
            float: left;
            padding-left: 0.25rem;
            box-sizing:border-box;
            -webkit-box-sizing: border-box;
        }

    }
   .login_img {
        padding-top: 1.1rem;
        box-sizing: border-box;
        -webkit-box-sizing: border-box;
        text-align: center;
    .xinli {
        display: block;
        width: 2.13rem;
        height: 2.02rem;
        margin: 0 auto;
    }
  }
  .login_ipt {
    width: 100%;
    margin: 0 auto;
    padding-top: 0.83rem;
    padding-left: 0.78rem;
    padding-right: 0.74rem;
    box-sizing: border-box;
    -webkit-box-sizing: border-box;
    text-align: center;
    li {
      width: 100%;
      height: 1.16rem;
      overflow: hidden;
      border-bottom: 1px solid #cfcfcf;
      padding-top: 0.58rem;
      box-sizing: border-box;
      -webkit-box-sizing: border-box;
      input::-webkit-input-placeholder {
        color: #b3b3b3;
      }
    }
    .phone {
      height: 1.16rem;
      font-size: 0.3rem;
      img {
        display: block;
        width: 0.27rem;
        float: left;
        margin: 0.1rem 0.4rem 0 0.33rem;
      }
      input {
        padding-top: 0.1rem;
        box-sizing: border-box;
        -webkit-box-sizing: border-box;
        display: block;
        width: 83%;
        color: #333333;
        float: left;
        border: 0;
        outline: none;
        background: #f8f8f8;
        font-size: 0.3rem;
      }
    }
    .pass {
      img {
        display: block;
        width: 0.27rem;
        float: left;
        margin: 0.1rem 0.4rem 0 0.33rem;
      }
      input {
        padding-top: 0.1rem;
        box-sizing: border-box;
        -webkit-box-sizing: border-box;
        display: block;
        width: 83%;
        color: #333333;
        float: left;
        border: 0;
        outline: none;
        background: #f8f8f8;
        font-size: 0.3rem;
      }
    }
    .verify {
        position: relative;
      img {
        display: block;
        width: 0.27rem;
        float: left;
        margin: 0.1rem 0.4rem 0 0.33rem;
      }
       input {
      padding-top: 0.1rem;
      box-sizing: border-box;
      -webkit-box-sizing: border-box;
      display: block;
      width: 83%;
      color: #333333;
      float: left;
      border: 0;
      outline: none;
      background: #f8f8f8;
      font-size: 0.3rem;
    }
     .verify_ri{
        width: 1.89rem;
        height: 0.62rem;
        border: 1px solid #ff8043;
        color: #ff8043;
        border-radius: 20px;
        position: absolute;
        left: 4rem;
        bottom: 0.2rem;
        background-color: #f8f8f8;

    }
     .count{
        width: 1.89rem;
        height: 0.62rem;
        border: 1px solid #999999;
        background-color: #999999;
        border-radius: 20px;
        color:#fff;
        font-size: 0.25rem;
        position: absolute;
        right: 0.09rem;
        margin-top: -.25rem;
     }
    }
     .read {
    padding-top: 0.35rem;
    box-sizing: border-box;
    -webkit-box-sizing: border-box;
    border:none;
    .read_img{
        float: left;
        width: 0.38rem;
        height: 0.38rem;
        margin-right: 0.2rem;
    }
    .read_le{
        display: block;
        float: left;
        font-size: 0.25rem;
        .read_ri{
            color: #ff8043;
        }
    }
  }  
  }
 
  .immider{
      width: 5.98rem;
      height: 0.9rem;
      background-color: #ff8043;
      border-radius: 20px;
      position: fixed;
      left: 50%;
      margin-left: -3rem;
      bottom: 1.7rem;
      color:#fff;
  }
}
</style>


