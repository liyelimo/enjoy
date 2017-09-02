<template>
	<div class="homecontent">
		<ul v-for = "item in proList" class="homecon" v-show ="listshow">
			<h3>{{item.group_section.title}}</h3>
			<div class="date">{{item.group_section.desc}}</div>
			<li v-for="per in classList" @click = "todetail(per.enjoy_url)">
				
					<img v-bind:src=per.url> 
					<div class="lititle">{{per.title}}</div>
					<div class="lidesc">{{per.desc}}</div>
				
			</li>
		</ul>
		<div class="city" v-show="cityshow">
			<div class="citytitle">本地服务开通城市</div>
			<ul class="cityname" >
				<li class="cityitem" v-for="item in list" @click = "tocity()">
					<router-link :to = "{name:'home',params:{cityid:item.id,proname:item.name}}">{{item.name}}</router-link>
				</li>
			</ul>
	</div>
</div>
</template>

<script>
	import MyAjax from "./../md/MyAjax.js";
	import router from "./../router/router.js";
	export default {
		data(){
			return {
				listshow:true,
				cityshow:false,
				proList:[],
				classList:[],
				cityid:104,
				city:"",
				list:[
				{name:'上海',id:104},
				{name:'北京',id:140},
				{name:'南京',id:144},
				{name:'天津',id:185},
				{name:'广州',id:216},
				{name:'成都',id:235},
				{name:'杭州',id:260},
				{name:'深圳',id:299},
				{name:'苏州',id:347},
				{name:'西安',id:362},
				{name:'重庆',id:388},
				{name:'长沙',id:401}
				]
				
			} 
		},
		watch:{
			'$route':function(newRoute){
				console.log(this.listshow,this.cityshow)
				
				this.listshow = false;
				this.cityshow= true;
				var that = this;
				console.log(newRoute);
				var cityid= newRoute.params.cityid;
				var url = "https://api.ricebook.com/hub/home/v1/web/week_choice.json?city_id="+cityid+"&page=0";
				MyAjax.vueJson(url,function(data){
					console.log(data)
					that.proList = data;
					for(var i in data){
						var list = data[i].tabs;
					}
					that.classList = list;
					
				},function(err){
					console.log(err)
				})
			}
		},
			mounted(){
				var that = this;
				var cityid = that.cityid;
				var proname = that.proname;
				var url = "https://api.ricebook.com/hub/home/v1/web/week_choice.json?city_id="+cityid+"&page=0";
				MyAjax.vueJson(url,function(data){
					console.log(data)
					that.proList = data;
					for(var i in data){
						var list = data[i].tabs;
					}
					that.classList = list;
				},function(err){
					console.log(err)
				});
				
			},
			methods:{
				todetail(data){
					var that = this;
					console.log(data);
					var arr = data.slice(29)
					console.log(arr);
					that.$router.push({path:'/detail',query:{allid:arr}});
					
				},
				tocity(){
					console.log(this.listshow);
					this.listshow = true;
				    this.cityshow= false;
					var that = this;
					var cityid = that.$route.params.cityid;
					console.log(cityid)
					var proname = that.$route.params.proname;
					
					localStorage.setItem('id',cityid);
					localStorage.setItem("name1",proname)
					
					$(".citynames").html(proname);
					var url = "https://api.ricebook.com/hub/home/v1/web/week_choice.json?city_id="+cityid+"&page=0";
					MyAjax.vueJson(url,function(data){
					console.log(data)
					that.proList = data;
					for(var i in data){
						var list = data[i].tabs;
					}
					that.classList = list;
					
				},function(err){
					console.log(err)
				})
				}
			}
		}
	
</script>

<style scoped>
	.homecontent{
		width: 90%;
		height: 90%;
		margin: 5%;
		overflow-y: auto;
		
	}
	.homecontent::-webkit-scrollbar {display: none;}
	.homecon h3{
		width: 100%;
		height: 20px;
		line-height: 20px;
		margin-top: 10px;
		font-size: 18px;
	}
	 .date{
	 	width: 100%;
	 	height: 20px;
	 	font-size: 12px;
	 	color: #f66;
	 }
	 .homecon li img{
	 	margin-top: 10px;
	 	margin-bottom: 10px;
	 	width: 100%;
	 	height: 190px;
	 }
	 .lititle{
	 	width: 100%;
	 	line-height: 20px;
	 	font-size: 14px;
	 	
	 }
	 .lidesc{
	 	width: 100%;
	 	height: 20px;
	 	line-height: 20px;
	 	font-size: 12px;
	 	color: #ccc;
	 }
	 .city{
	 	width: 100%;
	 	height: 100%;
	 }
	.citytitle{
		width: 100%;
		height: 20px;
		line-height: 20px;
		margin-top:10px ;
	}
	.cityname li{
		width: 30%;
		line-height: 30px;
		border: 1px solid #ccc;
		text-align: center;
		float:left;
		margin: 2% 1% ;
		font-size: 14px;
	
	}
</style>