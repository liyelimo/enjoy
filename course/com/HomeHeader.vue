<template>
	<div class="commonHeader">
		<div class="back" @click="gokind()">分类</div>
		<div class="title" @click="gowhere()">ENJOY<span class="citynames">{{proname}}</span><span><i class="iconfont">&#xe643;</i> </span></div>
		<div class="moreInfo">
			<i class="iconfont" @click="goLogin">登录</i>
			<i class="iconfont" @click = "gosearch()">&#xe651;</i>
		</div>
		<div class="searchbox" v-show = "auto">
			<input type="text" name="search" id="search" value="" placeholder="搜索本地精选/快递到家" />
			<span @click = "checksearch()">搜索</span>
		</div>
	</div>
</template>

<script>
	import "./../scss/main.scss";
	import router from "./../router/router.js";
	export default {
		data(){
			return {
				proname:"上海",
				auto:false,
				keyword:""
			}
		},
		methods:{
			gokind(){
				router.push({path:"prochoose"})
			},
			gowhere(){
				$(".city").toggle(),
				$(".homecon").toggle()
			},
			goLogin(){
				router.push({path:"login"})
			},
			gosearch(){
				var that = this;
			 	if(that.auto == false){
			 		that.auto = true;
			 	}else{
			 		that.auto = false;
			 	}
			},
			checksearch(){
				var that = this;
				that.keyword = $("#search").val();
				console.log(that.keyword);
				that.$router.push({path:"/search",query:{keyword:that.keyword}});
			}
		}
	}
</script>
	
<style scoped>
	.back{
		font-size: 14px;
	}
	.title{
		font-size: 14px;
	}
	.title span:nth-of-type(1){
		font-size: 8px;
	}
	.title i{
		font-style: inherit;
		
	}
	.moreInfo .iconfont{
			display: inline-block;
			font-size: 14px;
			width: 50px;
			float: left;
			font-style: inherit;
	}
	.searchbox{
		width: 100%;
		height: 25px;
		position: fixed;
		top: 50px;
		left: 0;
		z-index: 100;
		margin-bottom: 10px;
	}
	#search{
		text-indent: 12px;
		display: block;
		height: 25px;
		width: 70%;
		/*position: fixed;
		top: 52px;
		left:14%;*/
		margin-left: 10%;
		float: left;
		font-size: 14px;
		background-color: #ccc;
		border: none;
		outline:0;
		
	}
	.searchbox span{
		width: 10%;
		padding-left: 4%;
		display: inline-block;
		float: left;
		color: #000000;
		font-size: 16px;
		
	}
</style>