<template>
    <div class="mine">
        <div class="header">
            <div class="head_t">
                <a @click="back()"><img src="../../../static/images/h_return.png" alt=""></a>
                <span style="margin-right:.45rem;">{{mgs}}</span> 
            </div>
            <div class="white clearfix"><img src="../../../static/images/portrait.png" alt=""></div>
            <!-- <p class="per_one">老鼠爱上猫</p> -->
        </div>
       <div class="btn">
            <!-- <p class="per_thr">您成为最终9人</p>
            <p class="per_for">您战胜了2762人（新纪录）</p>
            <p class="per_five">您获得了3000点积分 </p> -->
            <p class="per_one">
                <span><i class="fa fa-user" aria-hidden="true"></i></span>
                参与人数：9999人
            </p>
            <p class="per_two">
                <img src="../../assets/qian.png" alt="">
                奖励积分:{{ points }}
                <img src="../../../static/images/gold.png" alt="">
            </p>
            <button @click="test()">确定</button>
       </div>
        <img class="btn_btn" src="../../../static/images/btn_btn.jpg" alt="">
    </div>
</template>
<script>

export default{
   data () {
      return {   
         mgs: '标题',
         points:'',
      }
    },
  components:{
      
  },
  methods: {
      test () {
          this.$router.push({path:'/test'})
      },
      back () {
          this.$router.go(-1)
      }
  },
  created () {
        this.axios({
            method:"get",
            url:this.$baseurl + "/api/answer/getOver",
            headers:{token:localStorage.getItem('token'),"Content-Type": "application/x-www-form-urlencoded"},
            params:{
                subjectReviewId: localStorage.getItem("subjectReviewId"),
            }
       }).then((res)=>{
            console.log(res);
            if(res.data.data.points == null){
              this.points = 0;
           }
       }).catch(err => {
         console.log(err);
       });
  }
}

</script>
<style lang="scss" scoped>

// 
.per_one{
     font-size: 0.4rem;
     color:#626262;
     font-weight: 700;
     text-align: center;
    //  margin-top: 1.52rem;
    }
  .per_two{
      font-size: 0.38rem;
      color:#2083d1;
       text-align: center;
       margin-top: 0.38rem;
      img{
          width: 0.45rem;
          height: 0.45rem;
          vertical-align: middle;
          margin-left: 0.21rem;
      }
  }
.mine {
  width: 100%;
  height: 100%;
  background-color: #e9e9e9;
  .header {
    width: 100%;
    height: 4.93rem;
    position: relative;
    .head_t {
      width: 100%;
      height: 2.16rem;
      line-height: 0.88rem;
      background-color: #2782d7;
      font-size: 0.4rem;
      color: #fff;
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
    .white {
      width: 2.13rem;
      height: 2.13rem;
      border-radius: 50%;
      background-color: #fff;
      position: absolute;
      top: -27px;
      bottom: 0;
      left: 0;
      right: 0;
      margin: auto;
      padding: 0.16rem;
      box-sizing: border-box;
      text-align: center;
      img {
        width: 1.82rem;
        height: 1.82rem;
        border-radius: 50%;
        float: left;
      }
    }
    .per_one{
     font-size: 0.4rem;
     color:#626262;
     font-weight: 700;
     text-align: center;
     margin-top: 1.52rem;
 }
  .per_two{
      font-size: 0.38rem;
      color:#2083d1;
       text-align: center;
       margin-top: 0.38rem;
      img{
          width: 0.45rem;
          height: 0.45rem;
          vertical-align: middle;
          margin-left: 0.22rem;
      }
  }
  }
  .btn{
        box-sizing: border-box;
        -webkit-box-sizing: border-box;
        position: relative;
      .per_thr{
            font-size: .40rem;
            color: #ff8043;
            text-align: center;
            font-weight: 700;
        }
        .per_for{
            margin-top: .38rem;
            font-size: .40rem;
            color: #ff8043;
            text-align: center;
        }
        .per_five{
            font-size: .40rem;
            color: #ff8043;
            text-align: center;
        }
  button{
      height: 0.9rem;
      width: 7rem;
      background-color: #ff8043;
      border-radius: 20px;
      color:#fff;
      font-size: 0.32rem;
      position: absolute;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      margin: auto;
      margin-top: 4.38rem;

  }

  }
  .btn_btn{
      position: fixed;
      bottom: 0;
  }
}
</style>
