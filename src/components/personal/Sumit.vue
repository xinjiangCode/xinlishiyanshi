<template>
	<div class="multiplayer">
    	<div class="header clearfix">
            <a @click="back()"><img src="../../../static/images/h_return.png" alt=""></a>
            <span style="margin-right:.45rem;">{{mgs}}</span> 
        </div>
		<div class="text">
			{{ title }}
			<!-- 广东省东莞成都 -->
		</div>
		<div class="text-form">
			<p v-for="item in items" :key="item.id">
				{{ item.text }}
			</p> 
			<p>{{ optionA }}</p>
			<p>{{ optionB }}</p>
			<p>{{ optionC }}</p>
			<p>{{ optionD }}</p>
			<!-- <p>二恶热</p>
			<p>吧v吧</p>
			<p>看就看</p>
			<p>欧珀哦</p> -->
		</div>

		<!-- <mt-swipe :show-indicators="false">
			<mt-swipe-item>1</mt-swipe-item>
			<mt-swipe-item>2</mt-swipe-item>
			<mt-swipe-item>3</mt-swipe-item>
		</mt-swipe> -->
		<button class="button" @click="oneOver()">
			提交
		</button>
  </div>
</template>

<script>
import { Swipe, SwipeItem } from 'mint-ui';
import { MessageBox } from 'mint-ui';
// Vue.component(Swipe.name, Swipe);
// Vue.component(SwipeItem.name, SwipeItem);
	export default {
  		name: 'Multiplayer',
  		data () {
		    return {
		      	mgs: '标题',
				msges: '[Did you see him yesterday?]属于下列哪种类型的句子？',
				title:'广东省东莞成都',
				optionA:'二恶热',
				optionB:'吧v吧',
				optionC:'看就看',
				optionD:'欧珀哦', 
		      	items: [
		      		// {id:1, text:"祈使句"},
		      		// {id:2, text:"疑问句"},
		      		// {id:3, text:"陈述句"},
		      		// {id:4, text:"感叹句"}
		      	]
		    }
		  },
		  methods: {
			  oneOver () {
				  this.$router.push({path:'/oneOver'});
				   MessageBox('提示', '提交成功');
			  },
			  back () {
				   this.$router.go(-1);
			  }
		  },
		  created () {
				this.axios({
					method:"get",
					url:this.$baseurl + "/api/answer/getSubject",
					headers:{token:localStorage.getItem('token'),"Content-Type": "application/x-www-form-urlencoded"},
					params:{
						subjectReviewId: 60,
						numbers:1
					}
			}).then((res)=>{
				console.log(res.data);
               this.items=res.data.data
			}).catch(err => {
				console.log(err);
			});
		}
		  
		}
</script>

<style lang="scss" scoped>
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
	.multiplayer-footer{
    width: 100%;
    height: .88rem;
    background-color: #2083d1;
    margin-bottom:12px;
    }
	.multiplayer-footer p{
		font-family: SourceHanSansCN-Regular;
		font-size: .36rem;
		font-weight: normal;
		font-stretch: normal;
		line-height: 0.88em;
		letter-spacing: 0px;
		color: #ffffff;
		text-align: center;
		padding-top: .26rem;
	}
	.text{
		padding-top: .58rem;
		width: 7.03rem;
		font-size: .36rem;
		color: #282828;
		margin: 0 auto;
		margin-bottom: .50rem;
		margin-top: 0.88rem;
	}
	.text-form{
     margin-bottom: 1.33rem;
	}
	.text-form p{
    font-size: .36rem;
		width: 4.40rem;
		height: .98rem;
		line-height: .98rem;
		background-color: #ffffff;
		border: solid 1px #91cb53;
		border-radius: 0.6rem;
		margin-bottom: .36rem;
		text-align: center;
		margin-left: .27rem;
	}
	.text-form p:nth-child(1){
		background-color: #91cb53;
		border: solid 1px #76b039;
	}
	.text-form p:nth-child(3){
		border: solid 1px #d35c4a;
		background-color: #ed7a68;
	}
	.button{
		width: 6.98rem;
		height: .89rem;
		background-color: #ff8043;
		border-radius: .45rem;
		margin-left: .24rem;
		margin-top: 1.44rem;
		border:none ;
		font-size: .36rem;
		color: #ffffff;
	}
</style>
