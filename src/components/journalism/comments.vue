<template>
    <div id="comments">
        <div class="header clearfix">
            <a @click="back()"><img src="../../../static/images/h_return.png" alt=""></a>
            {{mgs}}
        </div>
        <div class="comments_huise"></div>
        <scroller :on-refresh="refresh" :on-infinite="infinite" refresh-layer-color="#4b8bf4" loading-layer-color="#ec4949">
        <div class="comments_list">
            <ul>
                <li class="comments_list_li" v-for="item in items" :key="item.id">
                    <div class="list_buttom">
                        <img :src="item.imgUrl" alt="">
                        <span class="list_top_name">
                            <p class="list_top_name_age">{{ item.nickName }}</p>
                            <p class="list_top_name_time">{{ item.createTime | formatDate}}</p>
                        </span>
                        <span class="list_top_hui" @click="reply()">
                            <span><i class="fa fa-commenting" aria-hidden="true"></i></span>
                            回复信息
                        </span>
                    </div>
                    <div class="comments_text">
                        {{ item.content }}
                    </div>
                    <div class="comments_text_hui" style="font-size:.34rem">
                        <p>
                            <span class="comments_text_hui_one">回复</span>
                            <span class="comments_text_hui_two">类聚同意</span>
                            <span class="comments_text_hui_trr">2017-01-16      15:31</span>
                        </p>
                        <div class="comments_text_hui_text">
                            {{huifude}}
                        </div>
                    </div>
                </li>
            </ul>
        </div>
    </scroller> 
    </div>  
</template>
<script>
 export default {
    data () {
      return {   
        mgs:'评论',
        img: '',
        centent: '',
        createTime: '',
        nickName: '',
        huifude:'虽然乔帮主重新定义了手机，但是另一方面库克是让iphone走向世界的功臣，这点是不能被否定的。',
        items: [
            // {id:1,nickName:'2018',createTime:'2018-02-17',content:'ppp'}
        ],
        allload:0,
        allLength:1,
        page:'1',//当前
        limit:2,
        bottom:0,
      }
    },
    mounted(){
          this.jiekou();
    },
    methods: {
        back () {
            this.$router.go(-1);
        },
        reply () {
            this.$router.push({path:'/reply?newsId='+this.$route.query.newsId+'&memberId='+this.$route.query.newsId});
            // 评论回复接口
            this.$cfAjax1('post', '/api/news/saveComment',
                    'token', localStorage.getItem('token'), 
                    "newsId", this.$route.query.newsId,
                    "memberId",this.$route.query.newsId,
                    "content", "content",
                    function (res) {
                        console.log(res);
        
            });
        },

         jiekou () {
        // 获取此新闻评论接口
        this.axios.get(this.$baseurl + '/api/news/getCommentList',
        {
            params: {
                newsId: this.$route.query.newsId,
                page:this.page,
                limit: this.limit,
        	}
        }).then( res => {
            console.log(res);
            // this.items = res.data.page.list;  
            if(this.page==1){
                this.items=[];
                this.bottom=2;
                this.allLength = res.data.page.totalCount;
            }
            if(this.page*this.limit>=this.allLength){
                this.allload=1;
            }else{
                this.allload=0;
            }
            for (let i = 0; i < res.data.page.list.length; i++) {
                this.items.push(res.data.page.list[i]);
            }
               
 
        }).catch( err => {
            console.log(err);
        });

    
        //  获取评论回复接口
         this.axios.get(this.$baseurl + '/api/news/getCommentList',
        {
            params: {
                page:this.page,
                limit: this.limit,
                newsId: this.$route.query.newsId,
                commentId: 1,
        	}
        }).then( res => {
            console.log(res);   

        }).catch( err => {
            console.log(err);
        });

       
    },

        refresh(done) {
        var that=this;
        setTimeout(() => {
          that.page=1;
          that.bottom = 0;
          that.jiekou();
          done()
        }, 1500)
      },
      infinite(done) {
        var that = this;
        if (this.allload) {
          setTimeout(() => {
            done(true)
          }, 1500)
          return;
        }
        setTimeout(() => {        
          that.page++;
          that.jiekou()   
          setTimeout(() => {
            done()
          })
        }, 1500)
      },
    },
   
    // 时间格式转译
    filters: {
      formatDate: function (value) {
        let date = new Date(value);
        let y = date.getFullYear();
        let MM = date.getMonth() + 1;
        MM = MM < 10 ? ('0' + MM) : MM;
        let d = date.getDate();
        d = d < 10 ? ('0' + d) : d;
        let h = date.getHours();
        h = h < 10 ? ('0' + h) : h;
        let m = date.getMinutes();
        m = m < 10 ? ('0' + m) : m;
        let s = date.getSeconds();
        s = s < 10 ? ('0' + s) : s;
        return y + '-' + MM + '-' + d + ' ' + h + '-' + m + '-' + s;
      }
    }
 }
</script>
<style lang="scss" scoped>
    .comments_list_li 
    #comments{
        background-color: #fff;
    }
    .header{
        height: 0.88rem;
        line-height: 0.88rem;
        font-size:0.4rem;
        background-color: #2083d1;
        color:#fff;
        text-align: center;
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        z-index: 1;
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
    .comments_huise{
        height: 100%;
        height: .29rem;
        background-color: #e9e9e9;
    }
    .comments_list_li{
        border-bottom:1px solid #dadada;
        padding-bottom: .42rem;
        margin-top: .8rem;
    }
    .comments_list{
        background-color:#ffffff;
        // width:7.02rem;
        // height: 3.7rem;
        margin: 0 auto;
        padding-left: 0.24rem;
        padding-right: 0.24rem;
        box-sizing:border-box;
      -webkit-box-sizing: border-box;
    }
    .list_top{
        width: 100%;
        height: .75rem;
    }
    .list_top img{
        width: .75rem;
        height: .75rem;
    }
    .list_top_name{
        font-size: .36rem;
        color: #2083d1;
    }
    .list_top_hui{
        color: #9e9e9e;
        font-size: .24rem;
        text-align: right;
        float: right;
        margin-top: .4rem;
    }
    .list_buttom{
        font-size: .34rem;
        color: #333333;
    }
    .list_buttom img{
        width: .75rem;
        height: .75rem;
        margin-top: .2rem;
    }
    .list_top_name{
        display: inline-block;
    }
    .list_top_name_time{
        font-size: .24rem;
	    color: #c2c2c2;
    }
    .comments_text{
        width: 5.97rem;
        font-size: .34rem;
        line-height: .47rem;
        color: #333333;
        margin-left: .8rem;
        margin-top:.38rem;
    }
    .comments_text_hui_one{
        color: #7d7d7d;
        font-size: .34rem;
    }
    .comments_text_hui_two{
        color: #323232;;
        font-size: .34rem;
        margin-left: .2rem;
    }
    .comments_text_hui_trr{
        color: #c2c2c2;
        font-size: .24rem;
        margin-left: 1.28rem;
    }
    .comments_text_hui{
         margin-left: .8rem;
         margin-top: .8rem;
         width: 6.05rem;
    }
    .comments_text_hui_text{
        margin-top: .21rem;
        font-size: .34rem;
        color:#7d7d7d;
    }
</style>
