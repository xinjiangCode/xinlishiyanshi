<template>
    <div class="mine">
       <div class="header">{{mgs}}</div>
        <v-touch v-on:swipedown="onSwipeDown()" style="padding-top: 0.88rem">
        <div class="count_new">
            <div class="top_search" v-if="show2">
              <input type="text" placeholder="请输入搜索内容" v-model.trim="title">
              <img class="search_img" src="../../../static/images/search_img.png" alt="">
            </div>
             <ul class="study_study">
                  <li id="t1" class="study_color study_li_li" v-for="list in lists" :key="list.id" :class="list.id==id?'Istrue':'Isfalse'" @click="tabs(list.id)" >{{list.title}}</li>
              </ul>
              <scroller style="top:2.9rem;" :on-refresh="refresh" :on-infinite="infinite" refresh-layer-color="#4b8bf4" loading-layer-color="#ec4949">
                <div class="new_small  clearfix" v-for="(ranking,index) in rankings" :key="index"  @click="studyDatils(ranking.points)">
                  <div class="le_new"><img class="new_datu" :src="ranking.imgUrl" alt=""></div>
                  <div class="ri_new"> 
                    <p class="new_text">{{ranking.title}}</p>
                    <div class="new_div div_spall clearfix">
                      <p class="new_write">作者：{{ranking.author}}</p>
                      <p class="new_time">发布时间： {{ranking.createTime | formatDate}}</p>
                    </div>
                    <div class="rice_new  clearfix" v-for="(label,lindex) in ranking.labelList" :key="lindex"> {{label.labelName}} </div>
                    <div class="new_btn clearfix">
                      <div :class="ranking.points!=null?'btn_left':'btn_left isHide'">{{ranking.points}}</div>
                      <div class="btn_right">
                        <div class="bianji"><img class="new_xiaotu" src="../../../static/images/new_zhuanfa.jpg" alt="">{{ranking.shareCount}}</div>           
                      </div>
                    </div>
                  </div>
                </div>     
              </scroller>           
        </div>
        </v-touch>
       
        <Footerremind></Footerremind>
    </div>
</template>
<script>
import { MessageBox } from "mint-ui";

import Footerremind from "../account/footer.vue";

// 节流函数
const delay = (function() {
  let timer = 0;
  return function(callback, ms) {
    clearTimeout(timer);
    timer = setTimeout(callback, ms);
  };
})();

export default {
  data() {
    return {
      mgs: "研究室",
      show2: false,
      show1: false,
      // show:false,
      allload: 0,
      allLength: 1,
      page: "1", //当前
      limit: 2,
      bottom: 0,
      id:'',
      title:'',
      search:[],
      lists:[
        // {id:1,title:'测试1'},
        // {id:2,title:'测试2'},
        // {id:3,title:'测试3'},
        // {id:4,title:'测试4'},
      ],
      rankings: [
        // {
        //   id: 1,
        //   title: "2017年新米上市 中粮初萃 五常稻花香大米5kg",
        //   author: "花花",
        //   createTime: "2018-09-09",
        //   shareCount: "234",
        //   labelLists: [
        //     {
        //       labelId: 4,
        //       labelName: "新米"
        //     }
        //   ]
        // },
        // {
        //   id: 2,
        //   title: "2018年新米上市 中粮初萃 五常稻花香大米5kg",
        //   author: "大大",
        //   createTime: "2018-09-19",
        //   shareCount: "234",
        //   labelLists: [
        //     {
        //       labelId: 4,
        //       labelName: "新米"
        //     }
        //   ]
        // },
        // {
        //   id: 3,
        //   title: "2018年新米上市 中粮初萃 五常稻花香大米5kg",
        //   author: "大大",
        //   createTime: "2018-09-19",
        //   shareCount: "234",
        //   labelLists: [
        //     {
        //       labelId: 4,
        //       labelName: "新米"
        //     }
        //   ]
        // },
        // {
        //   id: 4,
        //   title: "2018年新米上市 中粮初萃 五常稻花香大米5kg",
        //   author: "大大",
        //   createTime: "2018-09-19",
        //   shareCount: "234",
        //   labelLists: [
        //     {
        //       labelId: 4,
        //       labelName: "新米"
        //     }
        //   ]
        // }
      ]
    };
  },
  components: {
    Footerremind
  },
   watch: {
  //watch title change
    title() {
      delay(() => {
        this.fetchData();
      }, 300);
    },
  },
  mounted() {
    this.jiekou();
    this.categories();
  },

  methods: {
     async fetchData() {
       this.page=1;
       this.jiekou()
    //   this.axios({
    //     url: this.$baseurl + "/api/data/getDataList",
    //     method: 'get',
    //     params: { title: this.title },
    //   }).then(res => {
    //       console.log(res);
    //  this.search = res.data.page.list;
    //     })
    //     .catch(err => {
    //       console.log(err);
    //     });
    },
    tabs(id) {
      this.id=id;
      this.jiekou();      
    },

    categories () {
          this.axios({
        method: "get",
        url: this.$baseurl + "/api/data/getDataTypes",
        params: {
                 }
      })
        .then(res => {
          console.log(res);
          this.lists=res.data.data;
          this.id=this.lists[0].id;

          this.jiekou();
        })
        .catch(err => {
          console.log(err);
        });
         },

    jiekou() {      
      console.log("created");
      this.axios({
        method: "get",
        url: this.$baseurl + "/api/data/getDataList",
        params: {
          page: this.page,
          limit: this.limit,
          types:this.id,
          titles: this.title,
        }
      })
        .then(res => {
          console.log(res);
          if (this.page == 1) {
            this.rankings = [];
            this.bottom = 2;
            this.allLength = res.data.page.totalCount;
          }
          if (this.page * this.limit >= this.allLength) {
            this.allload = 1;
          } else {
            this.allload = 0;
          }
          for (let i = 0; i < res.data.page.list.length; i++) {
            this.rankings.push(res.data.page.list[i]);
          }
          // console.log(this.bottom + "===============");
        })
        .catch(err => {
          console.log(err);
        });
    },
    refresh(done) {
      var that = this;
      setTimeout(() => {
        that.page = 1;
        that.bottom = 0;
        that.jiekou();
        done();
      }, 1500);
    },
    infinite(done) {
      var that = this;
      if (this.allload) {
        setTimeout(() => {
          done(true);
        }, 1500);
        return;
      }
      setTimeout(() => {
        that.page++;
        that.jiekou();
        setTimeout(() => {
          done();
        });
      }, 1500);
    },

    onSwipeDown: function() {
      this.show2 = true;
    },

    studyDatils(points) {
      if (points != null) {
        const tknr =
          '<div style="text-alige:center;height:1rem;line-height:1rem;">确定兑换该资料吗？</div>';
        MessageBox.confirm("", {
          message: tknr,
          confirmButtonText: "确定",
          cancelButtonText: "取消"
        })
          .then(action => {
            if (action == "confirm") {
              //确认的回调
              console.log(1);
              this.$router.push({ path: "/studyDatil?statu=1" });
            }
          })
          .catch(err => {
            if (err == "cancel") {
              //取消的回调
              console.log(2);
            }
          });
      } else {
        this.$router.push({ path: "/studyDatil?statu=0" });
      }
    }
  },
  filters: {
    formatDate: function(value) {
      let date = new Date(value);
      let y = date.getFullYear();
      let MM = date.getMonth() + 1;
      MM = MM < 10 ? "0" + MM : MM;
      let d = date.getDate();
      d = d < 10 ? "0" + d : d;
      let h = date.getHours();
      h = h < 10 ? "0" + h : h;
      let m = date.getMinutes();
      m = m < 10 ? "0" + m : m;
      let s = date.getSeconds();
      s = s < 10 ? "0" + s : s;
      return y + "-" + MM + "-" + d;
    }
  }
};
</script>
<style lang="scss" scoped>
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
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
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
  .count_new {
    .top_search {
      width: 100%;
      height: 0.89rem;
      background-color: #fff;
      position: relative;

      input {
        width: 7rem;
        height: 0.54rem;
        background-color: #f1f1f1;
        border-radius: 20px;
        position: absolute;
        top: 0.16rem;
        left: 0.24rem;
        outline: none;
        padding-left: 0.3rem;
        box-sizing: border-box;
        -webkit-box-sizing: border-box;
      }

      .search_img {
        width: 0.3rem;
        height: 0.3rem;
        position: absolute;
        top: 0.3rem;
        right: 0.5rem;
      }
    }
    .study_study {
      height: 1.12rem;
      padding-top: 0.3rem;
      padding-left: 0.25rem;
      padding-right: 0.25rem;
      box-sizing: border-box;
      -webkit-box-sizing: border-box;
      .study_color {
        border-radius: 27px;
        border: solid 1px #2083d1;
        float: left;
        width: 1.55rem;
        height: 0.53rem;
        line-height: 0.53rem;
        font-size: 0.25rem;
        text-align: center;
        color: #666666;
        background-size: contain;
        margin-right: 2.2%;
      }
      .Istrue{
        color:#fff;
        background-color: #2083d1;
        
      }
      .Isfalse{
        background-color: none;
      }
      .last {
        margin-right: 0;
      }
    }
    .new_small {
      width: 100%;
      height: 2.7rem;
      font-size: 0.2rem;
      background-color: #fff;
      padding-top: 0.33rem;
      padding-bottom: 0.33rem;
      padding-right: 0.25rem;
      box-sizing: border-box;
      -webkit-box-sizing: border-box;
      margin-bottom: 0.16rem;
      .le_new {
        float: left;
        width: 40%;
        text-align: center;
        .new_datu {
          width: 2.6rem;
          height: 1.97rem;
        }
      }
      .ri_new {
        width: 60%;
        float: right;
        .new_text {
          font-size: 0.26rem;
        }
        .new_div {
          padding-top: 0.2rem;
          padding-bottom: 0.2rem;
          box-sizing: border-box;
          -webkit-box-sizing: border-box;
          font-size: 0.2rem;
          color: #2083d1;
          .new_write {
            float: left;
          }
          .new_time {
            float: right;
          }
        }
         .rice_new {
          color: #e6456e;
          margin-bottom: 0.1rem;
          margin-right: 0.2rem;
          font-size: 0.23rem;
          float: left;
        }

        .new_btn {
          color: #666666;
          margin-top: 0.45rem;
          .zhuanfa {
            float: right;
            margin-right: 0.4rem;
            .new_xiaotu {
              width: 0.25rem;
              height: 0.25rem;
              margin-right: 0.15rem;
            }
          }
          .bianji {
            float: right;
            .new_xiaotu {
              width: 0.25rem;
              height: 0.25rem;
              margin-right: 0.15rem;
            }
          }
        }
        .btn_left {
          float: left;
          width: 1.34rem;
          height: 0.47rem;
          line-height: 0.47rem;
          background-color: #2083d1;
          border-radius: 2px;
          text-align: center;
          color: #fff;
          display: block;
        }
        .isHide {
          display: none;
        }
        .btn_right {
          float: right;
          margin-top: 0.1rem;
        }
      }
    }
  }
}
</style>
