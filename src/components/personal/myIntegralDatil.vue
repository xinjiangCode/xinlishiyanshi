<template>
    <div class="mine">
    <div class="header clearfix"><a @click="back()"><img src="../../../static/images/h_return.png" alt=""></a>{{mgs}}</div>
        <ul class="integral_name">
            <li class="name_time">时间</li>
            <li class="name_datil">积分明细</li>
            <li class="name_integral">积分</li>
        </ul>
        
        <ul class="integral_income" v-for="ranking in rankings" :key="ranking.id">
            <li class="income_time">{{ranking.createTime}}</li>
            <li class="income_datil">{{ranking.pointsFrom}}</li>
            <li :class="ranking.pointsNumber>0?'income_integral':'income_integral isRed'">{{ranking.pointsNumber}}</li>
        </ul>
        <!-- <ul class="integral_spending">
            <li class="spending_time">2018-03-04</li>
            <li class="spending_datil">积分兑换</li>
            <li class="spending_integral">-300</li>
        </ul>
         <ul class="integral_income">
            <li class="income_time">2018-03-04</li>
            <li class="income_datil">注册用户</li>
            <li class="income_integral">+30</li>
        </ul>
         <ul class="integral_income">
            <li class="income_time">2018-03-04</li>
            <li class="income_datil">分享朋友圈</li>
            <li class="income_integral">+30</li>
        </ul>
         <ul class="integral_spending">
            <li class="spending_time">2018-03-04</li>
            <li class="spending_datil">积分兑换</li>
            <li class="spending_integral">-300</li>
        </ul>
          <ul class="integral_income">
            <li class="income_time">2018-03-04</li>
            <li class="income_datil">注册用户</li>
            <li class="income_integral">+30</li>
        </ul> -->
       
    </div>
</template>
<script>

export default {
  data() {
    return {
         mgs:'我的积分详情',
		      	rankings: [
		      		// {id:1, createTime:"2018-03-04", pointsFrom:"分享朋友圈", pointsNumber: "+30"},
		      		// {id:2, createTime:"2018-03-04", pointsFrom:"积分兑换", pointsNumber: "-300"},
		      		// {id:3, createTime:"2018-03-04", pointsFrom:"注册用户", pointsNumber: "+30"},
		      		// {id:4, createTime:"2018-03-04", pointsFrom:"分享朋友圈", pointsNumber: "+30"},
		      		// {id:5, createTime:"2018-03-04", pointsFrom:"积分兑换", pointsNumber: "-300"},
		      		// {id:6, createTime:"2018-03-04", pointsFrom:"注册用户", pointsNumber: "+30"},
		      	]
    };
  },
  methods: {
      back () {
          this.$router.go(-1);
      },
  },
      created() {
    console.log("created"),
      this.axios({
          method:"get",
            url:this.$baseurl + "/api/member/getPointsList",
            headers:{token:localStorage.getItem('token'),"Content-Type": "application/x-www-form-urlencoded"},
            params:{
                //    page:1,
                //    limit:5,
                  }
       }).then((res)=>{
           console.log(res);
           this.rankings=res.data.data.list;
       }).catch(err => {
         console.log(err);
       });
     
  },

};
</script>
<style lang="scss" scoped>

.mine{
    width: 100%;
    height: 100%;
    background-color: #fff;
    
        .header{
        height: 0.88rem;
        line-height: 0.88rem;
        font-size:0.36rem;
        background-color: #2083d1;
        color:#fff;
        text-align: center;
        position: fixed;
        top:0;
        right: 0;
        left: 0;
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
    .integral_name{
        height: 1.08rem;
        font-size: 0.32rem;
        border-bottom: 1px solid #f3f3f3;
        margin-bottom: 0.4rem;
        margin-top: 0.88rem;
        li{
            width: 33.3333%;
            line-height: 1.08rem;
            float: left;
            text-align: center;
        }
    }
   
    .integral_income{
        height: 0.67rem;
        line-height: 0.67rem;
        text-align: center;
        font-size: 0.28rem;
        li{
            width: 33.3333%;
            float: left;
        }
         
         .income_integral{
                color:#2083d1;
            }
            .isRed{
            color:#ea8f46;
        }
    }
     .integral_spending{
        height: 0.67rem;
        line-height: 0.67rem;
        text-align: center;
        font-size: 0.28rem;
        li{
            width: 33.3333%;
            float: left;
        }
         .spending_integral{
                color:#ea8f46;
            }
    }
}

</style>
