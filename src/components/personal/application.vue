<template>
    <div class="mine">
       <div class="header clearfix">
         <a @click="back()"><img src="../../../static/images/h_return.png" alt=""></a>
         {{mgs}}
        </div>
        <ul class="applice">
          <li class="clearfix">
            <div class="applice_le">姓名</div>
            <div class="applice_ri"><input type="text" v-model="names" placeholder=""></div>
          </li>
          <li class="clearfix">
              <div class="applice_le">省份证号</div>
              <div class="applice_ri"><input type="text" v-model="ID" placeholder=""></div>
          </li>
          <li class="clearfix">
              <div class="applice_le">银行卡号</div>
              <div class="applice_ri"><input type="text" v-model="bank" placeholder=""></div>
          </li>
          <li class="clearfix">
              <div class="applice_le">开户行名称</div>
              <div class="applice_ri"><input type="text" v-model="opening" class="lastinput" placeholder=""></div>
          </li>
        </ul>
        <button @click="immediately()">立即领取</button>
        <div class="text_btn">
            <span class="title">积分兑换注意事项</span>
        <p class="content">
          这里是测试的文字这里是测试的文字这里是测试的文字这里是测试的文字这里是测试的文字
          这里是测试的文字这里是测试的文字这里是测试的文字这里是测试的文字这里是测试的文字
          这里是测试的文字这里是测试的文字这里是测试的文字这里是测试的文字这里是测试的文字
          这里是测试的文字这里是测试的文字这里是测试的文字这里是测试的文字这里是测试的文字
          </p>
          <span class="title">积分兑换注意事项</span>
        <p class="content">
          这里是测试的文字这里是测试的文字这里是测试的文字这里是测试的文字这里是测试的文字
          这里是测试的文字这里是测试的文字这里是测试的文字这里是测试的文字这里是测试的文字
          这里是测试的文字这里是测试的文字这里是
          </p>
        </div> 
    </div>
</template>
<script>

export default {
  data() {
    return {
      mgs:'申请兑换',
      names:'张三',
      ID:'421125299601012030',
      bank:'621126678987654567',
      opening:'中国银行霍营支行',
    };
  },
  methods: {
    back () {
      this.$router.go(-1);
    },
    immediately () {
      var that=this;
            if(this.ID == ''){
                this.$Toast({
                    message: '身份证号不能为空',
                    position: 'bottom'
                });
                return;
            }
            var ID=/(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/;
            if(!ID.test(this.ID)){
                this.$Toast({
                    message: '身份证号格式错误',
                    position: 'bottom'
                });
                return;
            }
                this.$Toast({
                    message: '已成功领取！',
                    position: 'bottom'});
              
              this.$cfAjax1('post','/api/member/payPrize',
                  'token',localStorage.getItem('token'),
                  'name', this.names, 
                  'idno', this.ID, 
                  'cardNumber', this.bank, 
                  'openingbank', this.opening, 
                  'prizeId', '1',  
                  function (res) {
                    console.log(res);
              });
      },
    }
  };
</script>
<style lang="scss" scoped>
.mine{
    width: 100%;
    height: 100%;
    background-color: #e9e9e9;
    position: relative;
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
    .applice{
      width: 100%;
      height: 1.10rem;
      background-color: #fff;
      font-size: 0.3rem;
      margin-top: 1.2rem;
       border-bottom: 1px solid #f3f3f3;
     li{
        width: 100%;
        height: 1.10rem;
        padding-left: 0.5rem;
       box-sizing: border-box;
      -webkit-box-sizing: border-box;
        background-color: #fff;
        .applice_le{
        float: left;
        width: 30%;
        height: 1.10rem;
        line-height: 1.10rem;

      }
      .applice_ri{
        float: right;
        width: 70%;
        height: 1.08rem;
        input{
             outline: none;
             width: 100%;
             height: 100%;
             border-bottom: 1px solid #f3f3f3;
              color:#2083d1;
        }
        .lastinput{
           border-bottom:none;
        }
         input::-webkit-input-placeholder{
              color:#2083d1;

      }
      }

     }
    }
    button{
      width: 6rem;
      height: 0.82rem;
      background-color: #ea8f46;
      color:#fff;
      font-size: 0.3rem;
      border-radius: 20px;
      position: absolute;
      left: 50%;
      margin-left: -3rem;
      margin-top: 3.8rem;
    }
    .text_btn{
      padding-left: 0.3rem;
      padding-right: 0.4rem;
       box-sizing: border-box;
      -webkit-box-sizing: border-box;
      color:#626262;
      font-size: 0.29rem;
      margin-top: 4.62rem;
      .title{
            color: #000;
          display: block;
          margin-bottom: 0.3rem;
          padding-top: 0.5rem;
      }
      .content{
        font-size: 0.22rem;
      }
    }
        
}

</style>
