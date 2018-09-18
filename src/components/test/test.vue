<template>
    <div class="mine">
          <div class="header">{{mgs}}</div> 
          <div class="head_b">
              <div class="test_head"></div>
               <ul class="test_datil clearfix">
                    <li class="top_one" @click="myIntegral()"><img src="../../../static/images/test_top1.png" alt=""><div class="test_text"><span class="test_num">1254</span><br><span class="text_test">实验积分</span></div></li>
                    <li @click="myAchievement()"><img src="../../../static/images/test_top2.jpg" alt=""><div class="test_text"><span class="test_num">629</span><br><span class="text_test">实验成就</span></div></li>
                    <li @click="myRanking()"><img src="../../../static/images/test_top3.jpg" alt=""><div class="test_text"><span class="test_num">23</span><br><span class="text_test">名次排行</span></div></li>
                </ul>
                <div class="white"><img :src="img" alt=""></div>
          </div>             
        <div class="answer_w">
            <div class="answer clearfix">
                <img class="answer_img" @click="solo()" src="../../../static/images/test_one.png" alt="">
                <div class="answer_text">
                    <!-- <p class="text_one" @click="solo()">单人<span class="dot"></span><span class="blue">答题</span></p> -->
                    <p class="text_one" @click="solo()">{{title1}}<span class="dot"></span><span class="blue">答题</span></p>
                    <p class="text_two">百科知识单人问答，知识竞赛</p>
                    <button @click="help()">帮助</button>
                </div>
            </div>
        </div>
       <div class="answer_w">
            <div class="answer clearfix">
                <img class="answer_img" @click="more()" src="../../../static/images/test_two.png" alt="">
                <div class="answer_text">
                    <!-- <p class="text_one" @click="more()">多人<span class="dot"></span><span class="blue">答题</span></p> -->
                    <p class="text_one" @click="more()">{{ title2 }}<span class="dot"></span><span class="blue">答题</span></p>
                    <p class="text_two">百科知识单人问答，知识竞赛</p>
                    <button  @click="help()">帮助</button>
                </div>
          </div>
          <div class="guding">进行中......</div>
       </div>
            <div class="answer_w">
                <div class="answer clearfix">
                <img class="answer_img" @click="morekun()" src="../../../static/images/test_more.png" alt="">
                <div class="answer_text">
                    <!-- <p class="text_one" @click="more()">多人<span class="dot"></span><span class="blue">答题困难</span></p>
                     -->
                     <p class="text_one" @click="morekun()">{{title3}}</p>
                    
                    <p class="text_two">百科知识单人问答，知识竞赛</p>
                    <button  @click="help()">帮助</button>
                </div>
            </div>
          <!-- <div class="guding">距离开始：1小时32分12秒</div> -->
          <div class="guding" v-show='flags==true'>距离开始：{{ time }}</div>
          <div class="guding" v-show='flags==false'>进行中...</div>
          </div>      
        <Foottest></Foottest>
    </div>
</template>
<script>
import Foottest from '../account/footer.vue'
export default{
    data () {
        return {   
            mgs:'实验室',
            img: '../../../static/images/portrait.png',
            title1:'',
            title2:'',
            title3:'',
            helps: '',
            startTime: '',
            time : '',
            flag : false,
            flags:true,
        }
    },
    components:{
        Foottest,
    },
    mounted () {
           let time = setInterval(()=>{
               if(this.flag == true){
                   clearInterval(time)
               }
               this.timeDown()
           },500)
       },
       props : {
           endTime : {
               type : String
           }
    },
    methods: {
        // 返回上一页
        back () {
            this.$router.go(-1);
        },
        // 单人答题页面
        solo () {
            this.$router.push({path:'/Solo?modeId='+1})
        },
        // 帮助页面
        help () {
            this.$router.push({path: '/help'})  
        },
        // 多人页面
        more () {
            this.$router.push({path: '/moreAwait?modeId='+2})
        },
        // 多人困难页面
        morekun () {
            this.$router.push({path: '/moreAwait?modeId='+3})
        },
        // 积分页面
        myIntegral () {
            this.$router.push({path: '/myIntegral'})
        },
        // 成就页面
        myAchievement () {
            this.$router.push({path: '/myAchievement'})
        },
        // 排行页面
        myRanking () {
            this.$router.push({path: '/Ranking'})
        },
        // 倒计时
        timeDown () {
            const endTime = new Date(this.endTimes);
            const nowTime = new Date();
            let leftTime = parseInt((endTime.getTime()-nowTime.getTime())/1000)
            //    let d = parseInt(leftTime/(24*60*60))
            let h = this.formate(parseInt(leftTime/(60*60)%24))
            let m = this.formate(parseInt(leftTime/60%60))
            let s = this.formate(parseInt(leftTime%60))
            if(leftTime <= 0){
                this.flag = true
                this.$emit('time-end')
            }
            //    this.time = `${d}天${h}小时${m}分${s}秒`
            this.time = `${h}小时${m}分${s}秒`
        },
        formate (time) {
            if(time>=10){
                return time
            }else{
                // return `0${time}`
                 return 0 
            }
        },
        if (time = 0) {
            this.flags==!this.flags;

        }
        
    },
    created() {
        console.log("created"),
        // 用户头像接口
        this.axios({
            method:"get",
            url:this.$baseurl + "/api/member/getImgUrl",
            headers:{token:localStorage.getItem('token'),"Content-Type": "application/x-www-form-urlencoded"},
                params:{
                      
                    }
        }).then((res)=>{
            console.log(res.data);
            this.img = res.data.data.imgUrl;
        }).catch(err => {
            console.log(err);
        });
        // 获取模式列表接口
        this.axios.get(this.$baseurl + '/api/answer/getModeList',
        	).then( res => {
                console.log(res.data);
                this.items = res.data.data.list;
                var itemss= this.items;
                this.title1 = itemss[0].title;
                this.title2 = itemss[1].title;
                this.title3 = itemss[2].title;
                this.helps = itemss[2].helps;
                // 获取开始时间
                this.startTime = itemss[2].startTime;
                // 获取结束时间
                this.endTimess = itemss[2].endTime;

                // 获取当前日期
                var now = new Date();
                var year = now.getFullYear(); //得到年份
                var month = now.getMonth();//得到月份
                var date = now.getDate();//得到日期
                month = month + 1;
                if (month < 10) month = "0" + month;
                if (date < 10) date = "0" + date;
                var time = "";
                    time = year + "-" + month + "-" + date;
                // 开始时间
                // var hetime = time + " "+ this.startTime;
                // var bhtime = hetime.replace(/-/g,'/'); 
                // console.log(bhtime);
                // 开始时间转换毫秒
                // var formatTimeS = new Date(bhtime).getTime();
                // console.log(formatTimeS);

                // 结束时间
                var endtimess = time + " "+ this.endTimess;
                var endtimes = endtimess.replace(/-/g,'/'); 
                // 结束时间转换毫秒
                this.endTimes = new Date(endtimes).getTime();
                
           	}).catch( err => {
           		console.log(err);
           	})  
    },
}

</script>
<style lang="scss" scoped>
.mine {
  width: 100%;
  height: 100%;
  background-color: #e9e9e9;
  .header {
    height: 0.88rem;
    line-height: 0.88rem;
    font-size:0.36rem;
    background-color: #2083d1;
    color:#fff;
    text-align: center;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 2;
      }
    .head_b {
      width: 100%;
      background-color: #fff;
      position: relative;
      top:0.88rem;
      .test_head{
        height: 1.28rem;
        background-color: #2083d1;
      }
      .test_datil{
        height: 2.77rem;
        background-color: #fff;
        font-size: 0.25rem;
        padding-top: 1.45rem;
        padding-left: 0.23rem;
        padding-right: 0.25rem;
        box-sizing: border-box;
        -webkit-box-sizing: border-box;
        text-align: center;
        li{
            width: 33%;
            float: left;
            font-size: 0.25rem;
            border-left: 1px solid #9c9c9c;
            img{
                // display: block;
                float: left;
                width: 0.7rem;
                height: 0.78rem;
                margin-right: 0.16rem;
                margin-left: 0.3rem;
            }
            .test_text{
                float: left;
                .test_num{
                    font-size: 0.3rem;
                    color: #2083d1;
                    font-weight:700;
                    // margin-bottom: 0.18rem;
                }
                .text_test{
                    color:#9c9c9c;
                }

            }
        }
        .top_one{
            margin-left: 0;
            border-left: none;
        }
    }
     .white {
      width: 2.13rem;
      height: 2.13rem;
      border-radius: 50%;
      background-color: #fff;
      position: absolute;
      top: -1.5rem;
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

    }

  .answer_w{
       position: relative;
       top:0.88rem;
      .answer{
        margin-top: 0.15rem;
        width: 100%;
        height: 2.13rem;
        background-color: #fff;
         padding-top: 0.23rem;
         padding-left: 0.22rem;
         padding-right: 0.25rem;
        padding-bottom: 0.18rem;
        box-sizing: border-box;
        -webkit-box-sizing: border-box;
       
        .answer_img{
            float: left;
            width: 2.47rem;
            height: 1.71rem;
            margin-right: 0.49rem;
        }
        .answer_text{
            float: left;
            font-size: 0.2rem;
            .text_one{
                font-size: 0.4rem;
                font-weight: 700;
                position: relative;
                .dot{
                    display: inline-block;
                    width: 0.1rem;
                    height: 0.1rem;
                    border-radius: 50%;
                    background-color: #000;
                    position: absolute;
                    top:0;
                    bottom: 0;
                    margin: auto;
                    margin-left: 2px;
                }
                .blue{
                    color: #2083d1;
                    margin-left: 0.2rem;
                }
            }
            .text_two{
                margin-bottom: 0.05rem;
                color: #9c9c9c;
            }
            button{
                width: 1.9rem;
                height: 0.5rem;
                border-radius: 20px;
                background-color: #ff8043;
                color:#fff;
                font-size: 0.3rem;
            }

        }       

    } 
     .guding{
            width: 100%;
            height: 0.5rem;
            line-height: 0.5rem;
            color:#fff;
            background-color: rgba(0, 0, 0,0.3);
            position: absolute;
            bottom: 0;
            font-size: 0.22rem;
            text-align: center;
        }

  }
}
</style>
