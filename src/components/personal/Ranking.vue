<template>
	<div class="ranking-list">
		<div class="header clearfix">
            <a @click="back"><img src="../../../static/images/h_return.png" alt=""></a>
            <span style="margin-right:.45rem;">{{mgs}}</span> 
        </div>
		<div class="ranking-button">
			<button id="b1" @click="lianSheng(),tabs(1)">连胜排行</button>
			<button id="b2" class="chaung_guan" @click="chuangG(),tabs(2)">闯关排行</button>
			<button id="b3" class="sheng_lv" @click="shengLv(),tabs(3)">胜率排行</button>
		</div>
		<div class="ranking-li" style="overflow: hidden;">
			<ul>
				<li class="ranking_list_li" v-for="ranking in rankings" :key="ranking.id">
					<span class="ranking-li-img">{{ ranking.imgUrl }}</span>
					<span class="ranking-li-text">{{ ranking.nickname }}</span>
					<span class="ranking-li-mine">当前排名</span>
					<span class="ranking-li-num">{{ ranking.ranking }}</span>
				</li>
			</ul>
		</div>
	</div>
</template>

<script>
	export default {
  		name: 'Ranking',
  		data () {
		    return {
		      	mgs: '排行榜',
		      	rankings: [
		      		// {id:1, text:"丰田宝来", img:"当时", num: 102},
		      		// {id:2, text:"丰田宝来", img:"当时", num: 102},
		      		// {id:3, text:"丰田宝来", img:"当时", num: 102},
		      		// {id:4, text:"丰田宝来", img:"当时", num: 102},
		      		// {id:5, text:"丰田宝来", img:"当时", num: 102},
		      		// {id:6, text:"丰田宝来", img:"当时", num: 102}
		      	]
		    }
		  },
		  mounted () {
			  this.jiekou();
		  },
		methods: {
			tabs(i){
				for(var j=1;j<=3;j++){
				if(j==i){
					document.getElementById('b'+j).style.backgroundColor = '#2083d1';
					document.getElementById('b'+j).style.color = '#fff';
				}else{
					document.getElementById('b'+j).style.background = 'none';
					document.getElementById('b'+j).style.color = ' #666666';
				}
				}
			},
			back () {
				this.$router.go(-1);
			},
			jiekou () {
			//获取连胜排行榜数据接口
				this.axios({
					method:"get",
					url:this.$baseurl + "/api/answer/getVictoryRankingList",
					headers:{token:localStorage.getItem('token'),"Content-Type": "application/x-www-form-urlencoded"},
				}).then( res => {
					console.log(res.data.data.list);
					this.rankings = res.data.data.list;
				}).catch( err => {
					console.log(err);
				});
			},
			// 连胜
			lianSheng () {
				// 获取连胜排行榜数据接口
				// debugger
				// this.axios({
				// 	method:"get",
				// 	url:this.$baseurl + "/api/answer/getVictoryRankingList",
				// 	headers:{token:localStorage.getItem('token'),"Content-Type": "application/x-www-form-urlencoded"},
				// }).then( res => {
				// 	console.log(res.data.data.list);
				// 	this.rankings = res.data.data.list;
				// }).catch( err => {
				// 	console.log(err);
				// });
				this.jiekou();
				console.log("bbbbbbbbbb");
			},
			
			// 胜率
			shengLv () {
				// 获取胜率排行榜数据接口
				this.axios({
					method:'get',
					url:this.$baseurl + '/api/answer/getRightCountRankingList',
					headers:{token:localStorage.getItem('token'),"Content-Type": "application/x-www-form-urlencoded"},	
				}).then( res => {
					console.log(res.data.data.list);
					this.rankings = res.data.data.list
				}).catch( err => {
					console.log(err);
				});
				console.log("cccccccccc")
			},
			// 闯关
			chuangG () {
				// 获取连胜排行榜数据接口
				this.axios({
					method:"get",
					url:this.$baseurl + "/api/answer/getRightCountRankingList",
					headers:{token:localStorage.getItem('token'),"Content-Type": "application/x-www-form-urlencoded"},
				}).then( res => {
					console.log(res.data.data.list);
					this.rankings = res.data.data.list;
				}).catch( err => {
					console.log(err);
				});
				console.log("aaaa");
			},
				
		}, 
	
	}
	
</script>

<style lang="scss" scoped>
	li:first-child .ranking-li-num{
		background-color: #ff8043;
		border: solid 2px #f05e18;
		color: #ffffff;
	}
	li:nth-child(2) .ranking-li-num{
		background-color: #ffd24a;
		border: solid 2px #fbc10a;
		color: #ffffff;
	}
	li:nth-child(3) .ranking-li-num{
		background-color: #c7c7c7;
		border: solid 2px #919191;
		color: #ffffff;
	}
	li:nth-child(4) .ranking-li-num{
		background-color: #e2a570;
		border: solid 2px #d57623;
		color: #ffffff;
	}
	.header{
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
	.ranking-list-text{
    	width: 100%;
    	height: .88rem;
    	background-color: #2083d1;
      	margin-bottom: -.88rem;
    }
	.ranking-list-text p{
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
  .ranking-li-mine{
    font-size:.3rem;
    color: #b9b9b9;
  }
	.ranking-button{
		display: flex;
		justify-content:center;
		text-align: center;
		margin-bottom: .44rem;
		// margin-top:.44rem; 
		margin-top: 1.35rem;
	}
	.ranking-button button{
    	font-size:.3rem;
		width: 1.99rem;
		height: .89rem;
		background-color: #ffffff;
		border:solid 1px #2782d7;
	}
	.ranking-button button:first-child{
		border-top-left-radius: 2em;
    	border-bottom-left-radius: 2em;
	}
	.ranking-button button:last-child{
		border-top-right-radius: 2em;
     	border-bottom-right-radius: 2em;
	}
	.ranking_list_li{
		width: 100%;
		height: 1.57rem;
    // line-height: 1.57rem;
    	line-height: 0.21rem;
		background-color: #ffffff;
		border: solid 1px #fbc10a;
		opacity: 0.4;
    	padding-left: .27rem;
	}
	.ranking-li-img{
		display: inline-block;
		border-radius:50%;
		width: 1.17rem;
		height: 1.17rem;
		background-color: #f8390a;
		line-height: 1.14rem;
		border: solid 4px #ff8043;
    text-align: center;
    font-size:.3rem;
	}
	.ranking-li-text{
		width: 1.18rem;
		height: .29rem;
		font-family: SourceHanSansCN-Medium;
		font-size: .30rem;
		font-weight: normal;
		font-stretch: normal;
		line-height: .21rem;
		letter-spacing: 0px;
		color: #000000;
		font-weight: 800;
	}
	.ranking-li-num{
		display: inline-block;
		width: .60rem;
		height: .60rem;
		background-color: #c4c4c4;
		border: solid 2px #939393;
		// opacity: 0.6;
		font-size: .24rem;
		color: #000000;
		border-radius: 50%;
		text-align: center;
		line-height: .60rem;
		// float: right;
		margin-right:.55rem;
		margin-top: .48rem;
	}
</style>
