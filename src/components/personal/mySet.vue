<template>
     <div class="mine">
        <div class="header clearfix"><a @click="back()"><img src="../../../static/images/h_return.png" alt=""></a>{{mgs}}</div>
           <ul class="set_top">
               <li class="set_img clearfix">
                   <div class="set_le"><img :src="imgUrl" alt=""></div>
                   <div class="set_fr" ><img src="../../../static/images/m_return.png" alt=""></div></li>
               <li class="clearfix" >
                   <input class="set_le" type="text"  v-model="name"  value="" :disabled="disabled1" placeholder="请输入您的昵称">
                    <button class="save" v-if="show1" @click="baocun1">保存</button>
               </li>
               <li class="clearfix">
                   <input class="set_le" type="text" v-model="city"  value="" :disabled="disabled2" placeholder="请输入您所在城市">
                    <button class="save" v-if="show2" @click="baocun2">保存</button>
                   </li>
               <li class="clearfix">
                   <input class="set_le" type="text" maxlength="11" v-model="phone"  value="" :disabled="disabled3"  placeholder="请输入您的手机号">
                    <button class="save" v-if="show3" @click="baocun3">保存</button>
                   </li>
           </ul>
           <ul class="set_btn">
              <li class="clearfix">
                  <div class="set_le">字体设置</div><div class="set_fr">
                      <img src="../../../static/images/m_return.png" alt="">
                  </div>
                </li>
              <li class="clearfix" @click="passwordsz()">
                    <div class="set_le">密码设置</div>
                    <div class="set_fr"><img src="../../../static/images/m_return.png" alt=""></div>
                </li>
           </ul>
            <button class="guding" @click="exit()">退出登录</button>
            <div class="page-actionsheet-wrapper">
                <button class=" button_opcity mint-button mint-button--default mint-button--large" @click="actionSheet"></button> 
            </div>
     <mt-actionsheet :actions= "data" v-model="sheetVisible"></mt-actionsheet>        
    </div>
    
</template>
<script>
import { Actionsheet } from "mint-ui";

export default {
  data() {
    return {
      mgs: "我的设置",
      show1: false,
      show2: false,
      show3: false,
      disabled1:false,
      disabled2:false,
      disabled3:false,
      phone: "",
      name: "",
      city: "",
      imgUrl:'../../../static/images/portrait.png',
      data: [
        {
          name: "拍照",

          method: this.getCamera // 调用methods中的函数
        },
        {
          name: "从相册中选择",
          method: this.getLibrary // 调用methods中的函数
        }
      ],
      //   action sheet 默认不显示，为false。操作sheetVisible可以控制显示与隐藏
      sheetVisible: false
    };
  },  
  
    //用户信息接口
    created() { 
    console.log("created"),
      this.axios({
          method:"get",
           url:this.$baseurl + "/api/member/getMemberInfo",
           headers:{token:localStorage.getItem('token'),"Content-Type": "application/x-www-form-urlencoded"},
       }).then((res)=>{
           console.log(res);
           this.phone = res.data.member.phone;
           this.name = res.data.member.nickname;
           this.city = res.data.member.area;
       }).catch(err => {
         console.log(err);
       });
     
  },
  watch: {
    name: function() {
      this.changeName();
    },
    city: function() {
      this.changeCity();
    },
    phone: function() {
      this.changePhone();
      // this.phone = this.phone.replace(/\D/g, "");    
    }
  },
  
  methods: {
    actionSheet: function() {
      this.sheetVisible = true;
    },
    getCamera: function() {
      console.log("打开照相机");
    },
    getLibrary: function() {
      console.log("打开相册");
    },
    baocun1: function() {
        console.log(this.name)
       if(this.name == undefined || null){
            this.show1 = true;
            this.disabled2 = true;
            this.disabled3 = true;
            this.$Toast({
                message: '昵称不能为空',
                position: 'bottom'
          }); 
            return;  
        }

      console.log("created"),
      this.axios({
          method:"post",
           url:this.$baseurl + "/api/member/saveMemberInfo",
           headers:{token:localStorage.getItem('token'),"Content-Type": "application/x-www-form-urlencoded"},
            params:{
                    nickname:this.name,
                }
       }).then((res)=>{
           console.log(res);  
           var code = res.data.code;    
           if(code == "0"){
              this.$Toast({
                      message: '保存成功',
                      position: 'bottom'
                  });   
            this.show1 = false;
            this.disabled1 = false;
            this.disabled2 = false;
            this.disabled3 = false;
       }
         
       }).catch(err => {
         console.log(err);
       });
    },
    baocun2: function() {
     
     if(this.city == undefined || null){
            this.show2 = true;
            this.disabled1 = true;
            this.disabled3 = true;
            this.$Toast({
                message: '城市不能为空',
                position: 'bottom'
          }); 
            return;  
        }

      console.log("created"),
      this.axios({
          method:"post",
           url:this.$baseurl + "/api/member/saveMemberInfo",
           headers:{token:localStorage.getItem('token'),"Content-Type": "application/x-www-form-urlencoded"},
            params:{
                    area:this.city,
                }
       }).then((res)=>{
           console.log(res);
          var code = res.data.code;    
           if(code == "0"){
            this.$Toast({
                      message: '保存成功',
                      position: 'bottom'
                  }); 
            this.show2 = false;
            this.disabled1 = false;
            this.disabled2 = false;
            this.disabled3 = false;
        }
           
       }).catch(err => {
         console.log(err);
       });
    },
    baocun3: function() { 
     
     if(this.phone == undefined || null){
            this.show3 = true;
            this.disabled2 = true;
            this.disabled1 = true;
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

      console.log("created"),
      this.axios({
          method:"post",
           url:this.$baseurl + "/api/member/saveMemberInfo",
           headers:{token:localStorage.getItem('token'),"Content-Type": "application/x-www-form-urlencoded"},
           params:{
                    phone:this.phone,
                }
       }).then((res)=>{  
          console.log(res);
          var code = res.data.code;    
           if(code == "0"){
              this.$Toast({
                      message: '保存成功',
                      position: 'bottom'
                  }); 
              this.show3 = false;
              this.disabled1 = false;
              this.disabled2 = false;
              this.disabled3 = false;
        }
           
       }).catch(err => {
         console.log(err);
       });
    },
    changeName: function() {
     
      var that = this;
      if (that.name != '') {
        that.show1 = true;
        that.disabled2=true
        that.disabled3=true
      } else {
        that.show1 = false;
        that.disabled2=false;
        that.disabled3=false;
        that.disabled1=false;
      }
    },
    changeCity: function() {

      var that = this;
      if (that.city != '') {
        that.show2 = true;
        that.disabled1=true
        that.disabled3=true
      } else {
        that.show2 = false;
        that.disabled2=false;
        that.disabled3=false;
        that.disabled1=false;
      }
    },
     changePhone: function() {

      var that = this;
       this.phone=this.phone.replace(/\D/g,''); 
       if (that.phone != '') {
        that.show3 = true;
        that.disabled2=true
        that.disabled1=true
      }
      else {
        that.show3 = false;
        that.disabled2=false;
        that.disabled3=false;
        that.disabled1=false;
        
      }
    },
    back () {
        this.$router.go(-1);
    },
    passwordsz () {
        this.$router.push({path: '/password'})
    },
     exit () {
        console.log("created"),
      this.axios({
          method:"post",
           url:this.$baseurl + "/api/member/logout",
           headers:{token:localStorage.getItem('token'),"Content-Type": "application/x-www-form-urlencoded"},
       }).then((res)=>{
           console.log(res);
          this.$router.push({path: '/weixin_login'})
       }).catch(err => {
         console.log(err);
       });
    },
   },   
   

  }
</script>
<style lang="scss" scoped>
.button_opcity {
  opacity: 0;
  width: 0.3rem;
  height: 0.3rem;
  position: absolute;
  top: 1.8rem;
  right: 0.2rem;
}
// input::-webkit-input-placeholder{color:#000;}
.save {
  width: 1.2rem;
  height: 0.5rem;
  background-color: #ff8043;
  border-radius: 20px;
  color: #fff;
  font-size: 0.24rem;
  position: absolute;
  top: 0.2rem;
  right: 0;
}
.mine {
  width: 100%;
  height: 100%;
  background-color: #e9e9e9;
  .header {
    height: 0.88rem;
    line-height: 0.88rem;
    font-size: 0.36rem;
    background-color: #2083d1;
    color: #fff;
    text-align: center;
    z-index: 1;
    position: fixed;
    top:0;
    right: 0;
    left: 0;
    a {
      display: block;
      width: 0.4rem;
      height: 0.5rem;
      float: left;
      padding-left: 0.25rem;
      box-sizing: border-box;
      -webkit-box-sizing: border-box;
    }
  }
  .set_top {
    margin-top: 1.08rem;
    padding-left: 0.24rem;
    padding-right: 0.24rem;
    box-sizing: border-box;
    -webkit-box-sizing: border-box;
    background-color: #fff;
    .set_img {
      height: 1.6rem;
      line-height: 0;
      .set_le {
        margin-top: 0.5rem;
        img {
          width: 0.8rem;
          height: 0.8rem;
        }
      }
      .set_fr {
        float: right;
        img {
          margin-top: 0.7rem;
        }
      }
    }
    li {
      height: 0.89rem;
      line-height: 0rem;
      border-bottom: 1px solid #f3f3f3;
      position: relative;
      .set_le {
        float: left;
        width: 70%;
        height: 0.89rem;
        line-height: 0.89rem;
        font-size: 0.28rem;
        outline: none;
        background-color: #fff;
      }
      .set_fr {
        float: right;
        img {
          width: 0.2rem;
          height: 0.3rem;
          margin-bottom: 0.2rem;
        }
      }
    }
  }
  .set_btn {
    margin-top: 0.23rem;
    padding-left: 0.24rem;
    padding-right: 0.24rem;
    box-sizing: border-box;
    -webkit-box-sizing: border-box;
    background-color: #fff;
    li {
      height: 1.1rem;
      line-height: 0rem;
      border-bottom: 1px solid #f3f3f3;
      .set_le {
        float: left;
        font-size: 0.28rem;
        margin-top: 0.45rem;
      }
      .set_fr {
        float: right;
        img {
          width: 0.2rem;
          height: 0.3rem;
          margin-bottom: 0.2rem;
        }
      }
    }
  }
  .guding {
    width: 6rem;
    height: 0.8rem;
    background-color: #ff8043;
    color: #fff;
    border-radius: 20px;
    position: fixed;
    left: 50%;
    margin-left: -3rem;
    bottom: 0.2rem;
    font-size:.36rem;
  }
}
</style>

