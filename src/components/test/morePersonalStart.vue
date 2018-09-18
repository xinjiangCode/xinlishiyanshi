<template>
	<div class="multiplayer">
    	<div class="head_t">
          <span @click="goBack()" class="span11"><img src="../../../static/images/h_return.png" alt=""></span>
          <span style="margin-right: .45rem;">{{msg}}</span> 
      </div>
      <div class="top">
            <img src="../../../static/images/clcok.jpg" alt="" class="top_img_jpg">
            <!-- <p class="top_num">20/78人</p> -->
            <hr class="top_hr">
            <img src="../../../static/images/morepersonal.png" alt="" class="top_img_png">
        </div>
        <div v-for="item in subjects" :key="item.id">
      <div class="text">
        {{ item.msges }}
      </div>
      <div class="text-form12" >
          <div class="xuanxiang" v-for="(item1,idx) in item.items" :key="item1.id" @click="seleAnwer(item1.id,suId,idx,item.anwer)" :class="item.isYourAnser=='-1'?'':item.anwer==item1.id?'green' :item.isYourAnser==item1.id?'red' :''">
            {{ item1.text }}
          </div>
      </div>
      <div class="btns">
        <div class="cf_btn " v-show="next == false" @click="goForward()">上一题</div>
        <div class="cf_btn " @click="goNext()" v-if="page=subjects.length">下一题</div>
        <div class="cf_btn " v-if="page==1" @click="goBack()">返回</div>
        <div class="cf_btn " @click="oneOver()" style="background-color: #ff8043;" v-show="next == false">提交</div>
      </div>
    </div>
  </div>
</template>

<script>
import { MessageBox } from "mint-ui";
export default {
  name: "Multiplayer",
  data() {
    return {
      msg: "标题",
      page: 1, //当前题码   
      subjectReviewId:'',
      numbers:1,
      suId:'',
      anwer:'',
      id:'',
      options:'',
      next:true,
      flge:true,
      subjects: [

      ]
    };
  },
  created() {
    // 第一次初始化题目和选项接口
    this.axios({
      method: "get",
      url: this.$baseurl + "/api/answer/getSubject",
      headers: {
        token: localStorage.getItem("token"),
        "Content-Type": "application/x-www-form-urlencoded"
      },
      params: {
        modeId: 2
      }
    })
      .then(res => {
        // console.log(res);
        console.log(res.data.data);
        this.subjectReviewId = res.data.data.subjectReviewId;
        var datas = res.data.data;
        this.options = datas.options;
        this.numbers = res.data.data.numbers;
        this.suId = res.data.data.id;
        this.subjects = [
          {
            isYourAnser: datas.options == null ? '-1' : datas.options,
            id: datas.id,
            numbers: datas.numbers,
            anwer: datas.optionRight == null ? 0 : datas.optionRight,
            msges: datas.title,
            items:[
              { id: 1, text: datas.optionA },
              { id: 2, text: datas.optionB },
              { id: 3, text: datas.optionC },
              { id: 4, text: datas.optionD }
            ]
          }
        ];
          //保存subjectReviewId 和 id 到 localstorage
          var subjectReviewId=res.data.data.subjectReviewId;
          localStorage.setItem('subjectReviewId', subjectReviewId);
          var id=res.data.data.id;
          localStorage.setItem('id', id);
      })
      .catch(err => {
        console.log(err);
      });
  },
  methods: {
    // 跳到test页面
    goBack() {
      // this.$router.go(-1);
      this.$router.push({path:'/test'})
    },
    // 下一题
    goNext() {
        var that = this;
        this.axios({
            method: "get",
            url: this.$baseurl + "/api/answer/getSubject",
            headers: {
                token: localStorage.getItem("token"),
                "Content-Type": "application/x-www-form-urlencoded"
            },
            params: {
                numbers:Number(localStorage.getItem("numbers"))+1,
                subjectReviewId: localStorage.getItem("subjectReviewId"),
            }
        })
      .then(res => {
        console.log(res.data.data);
        localStorage.setItem('numbers', Number(localStorage.getItem("numbers"))+1);
        var datas = res.data.data;
        // this.suId = res.data.data.id;
        this.options = datas.options
        this.next = datas.next
        //true
        // 下一题
        that.page = that.page+1;
        // 没有下一题时 return

        if (this.next == "flase") { 
          return;
        }
        // 选项加入subjects数组
        this.subjects = [
          {
            // 判断答案正确与否
            isYourAnser: datas.options == null ? '-1' : datas.options,
            options: datas.options,
            id: datas.id,
            numbers: datas.numbers,
            anwer: datas.optionRight == null ? 0 : datas.optionRight,
            msges: datas.title,
            items:[
              { id: 1, text: datas.optionA },
              { id: 2, text: datas.optionB },
              { id: 3, text: datas.optionC },
              { id: 4, text: datas.optionD }
            ]
          }
        ];
      })
      .catch(err => {
        console.log(err);
      });
    },
    //上一题
    goForward() {
        var that = this;
        this.axios({
            method: "get",
            url: this.$baseurl + "/api/answer/getSubject",
            headers: {
                token: localStorage.getItem("token"),
                "Content-Type": "application/x-www-form-urlencoded"
            },
            params: {
                numbers:Number(localStorage.getItem("numbers"))-1,
                subjectReviewId: localStorage.getItem("subjectReviewId"),
            }
        })
      .then(res => {
        var datas = res.data.data;
        this.suId = res.data.data.id;
        localStorage.setItem('numbers', Number(localStorage.getItem("numbers"))-1);
        // 上一题
        that.page = that.page-1;
        // 没有下一题时 return
        // if (that.page == that.subjects.length) { 
        //   return;
        // }
        // 选项加入subjects数组
        this.subjects = [
          {
            // 判断答案正确与否
            
            options: datas.options,
            id: datas.id,
            
            numbers: datas.numbers,
            anwer: datas.optionRight,
            msges: datas.title,
            items:[
              { id: 1, text: datas.optionA },
              { id: 2, text: datas.optionB },
              { id: 3, text: datas.optionC },
              { id: 4, text: datas.optionD }
            ]
          }
        ];
         return
      })
      .catch(err => {
        console.log(err);
      });
     
    },
    
    // 选项
    seleAnwer(idx1,suId,idx,suerAmwer) {
      // debugger
      if(suerAmwer-1==idx){
          MessageBox.alert('您选对了').then(action => {
              this.$router.push({path:'/DianOver?id=' + localStorage.getItem("id")}); 
          }) 
      }else{
          MessageBox.alert('您被淘汰了').then(action => {
              this.$router.push({path:'/twoOver'}); 
          }) 
      }

      if (this.subjects[0].isYourAnser != "-1") {

        // this.$router.push({
        //     path: "/DianOver?id=" + suId
        // });  
        return;
      } else {
          this.subjects[0].isYourAnser = idx1;
          
      }     
    },
    // 提交
    oneOver(subjectReviewId) {
      if (this.subjects[0].length == "") {
          return
      }
      this.$router.push({
        path: "/twoOver"
      });
      MessageBox("提示", "提交成功");

    }
  }
};
</script>

<style scoped>
.cf_btn {
  width: 3.18rem;
  height: 0.89rem;
  background-color: #2083d1;
  border-radius: 0.45rem;
  border: none;
  margin-left: 0.24rem;
  margin-right: 0.3rem;
  font-size: 0.36rem;
  font-weight: normal;
  font-stretch: normal;
  line-height: 0.42rem;
  color: #ffffff;
  margin-bottom: 0.43rem;
  text-align: center;
  line-height: 0.89rem;
  float: left;
}
.head_t {
  width: 100%;
  line-height: 0.88rem;
  background-color: #2782d7;
  font-size: 0.4rem;
  color: #fff;
  text-align: center;
  position: fixed;
    top: 0;
    left: 0;
    right: 0;
}
.span11 {
  display: block;
  width: 0.4rem;
  height: 0.5rem;
  float: left;
  padding-left: 0.25rem;
  box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
.multiplayer-footer p {
  font-family: SourceHanSansCN-Regular;
  font-size: 0.36rem;
  font-weight: normal;
  font-stretch: normal;
  line-height: 0.88em;
  letter-spacing: 0px;
  color: #ffffff;
  text-align: center;
  padding-top: 0.26rem;
}
.text {
  width: 7.03rem;
  height: 0.97rem;
  font-family: MicrosoftYaHei;
  font-size: 0.36rem;
  font-weight: normal;
  font-stretch: normal;
  letter-spacing: 0px;
  color: #282828;
  margin: 0 auto;
  margin-bottom: 0.5rem;
}

.text-form12 div {
  font-size: 0.36rem;
  width: 4.4rem;
  height: 0.98rem;
  line-height: 0.98rem;
  background-color: #ffffff;
  border: solid 1px #91cb53;
  border-radius: 0.6rem;
  margin-bottom: 0.36rem;
  text-align: center;
  margin-left: 0.27rem;
}
.text-form12 .green {
  background-color: #91cb53;
  border: solid 1px #76b039;
}
.text-form12 .red {
  border: solid 1px #d35c4a;
  background-color: #ed7a68;
}
.multiplayer-button {
  width: 3.18rem;
  height: 0.89rem;
  background-color: #2083d1;
  border-radius: 0.45rem;
  border: none;
  margin-left: 0.24rem;
  margin-right: 0.3rem;
  font-size: 0.36rem;
  font-weight: normal;
  font-stretch: normal;
  line-height: 0.42rem;
  color: #ffffff;
  margin-bottom: 0.43rem;
  text-align: center;
  line-height: 0.89rem;
  float: left;
}
/*.multiplayer-button:first-child{*/
/*color: #e9e9e9;*/
/*background-color: red;*/
/*}*/
/*.multiplayer-button:last-child{*/
/*background-color: #ff8043;*/
/*}*/


.top{
      width: 100%;
      height: 3.86rem;
      background-color: #fff;
      padding-left: 0.24rem;
      margin-top:1.1rem;
  }
  .top_img_jpg{
    height: 1.11rem;
    margin: 0 auto;
    display: inherit;
    padding-right: .45rem;
}
.top_num{
	font-size: .36rem;
	line-height: .21rem;
	color: #2083d1;
}
.top_hr{
    width: 7.02rem;
	height: 1px;
	background-color: #d7d7d7;
    margin-top: .23rem;
}
.top_img_png{
    width: 7.02rem;
	height: 1.81rem;
}
</style>
