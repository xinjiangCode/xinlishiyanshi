<template>
    <div class="mine">
        <div class="header clearfix">
            <a class="router_link" @click="back()">
                <img src="../../../static/images/h_return.png" alt="">
            </a>
            {{msg}}
        </div>
        <div class="conter">
            {{text}}
            <!-- 这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字
            这里是测试用的文字这里是测试用的文字这里是测试用的文字这里是测试用的文字 -->

        </div>
    </div>
</template>
<script>
export default {
  data () {
     return {
       msg:'关于我们',
       text:'',
      
     }   
  },
 created () {
      console.log('created');
        this.axios.get(this.$baseurl + '/api/config/getConfig',
        	{
        		params: {
        			param: 'aboutUs'     		
        		}
        	}
        ) .then( res => {
                   console.log(res);
                   this.text=res.data.aboutUs
           		
           	}).catch( err => {
           		console.log(err);
           	})
        },

  watch:{ },
  methods: {
      back () {
          this.$router.go(-1)
      }
  },

   
} 
</script>
<style lang="scss" scoped>
.mine{
    width: 100%;
    height: 100%;
    background: #fff;
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
        .router_link{
            display: block;
            width: 0.4rem;
            height: 0.5rem;
            float: left;
            padding-left: 0.25rem;
            box-sizing:border-box;
            -webkit-box-sizing: border-box;
        }

    }
    .conter{
        font-size: 0.28rem;
        padding: 1.28rem 0.4rem 0.4rem 0.4rem;
        box-sizing:border-box;
        -webkit-box-sizing: border-box;
    }
}

</style>

