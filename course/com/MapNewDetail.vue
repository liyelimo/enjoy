<template>
	<div class="homecontent">
		 <div class="swiper-container">
	        <div class="swiper-wrapper">
	            <div class="swiper-slide" v-for = "item in imgs">
	            	<img :src=item.img_url>
	            </div>
	        </div>
        	<div class="swiper-pagination"></div>
    	</div>
    <div class="con">
    <div class="detitle" ><span>{{name1}}</span><span>-{{spec1}}</span></div>	
    <div class="detitle2">{{description1}}</div>
    <div class="deprice"><span>￥{{price1/100}}</span>/<span>{{show_entity_name}}</span><span>￥{{origin_price1/100}}</span></div>
    <div class="msg">
    	<div class="msgtitle">商户信息</div>
    	<div class="didian">{{restaurant_address1}}</div>	
    	<div class="num">{{restaurant_phone_numbers}}</div>
    </div>
    <div class="menu">
    	<div class="menutitle">MENU</div>
    	<div v-for = "per in contents" class="mainfood">
	    	<div>{{per.sub_title}}</div>
	    	<div class="text" v-for="item in per.text">{{item}}</div>
    	</div>
    </div>
    
    
    
    <div class="nn">亮点</div>
    <div class="bright" v-for="item in lights">
    	<img :src=item.img_url>
    	<div class="brtitle">{{item.title}}</div>
    	<div class="brinfo">{{item.content}}</div>
    </div>
    
    
    <div class="useinfo">
    	<div class="usetitle">使用提示</div>
    	<div class="usetext" v-for="item in contents1">{{item.text}}</div>
    </div>
    
    
    <div class="guess">
    	<div class="guesstitle">猜你喜欢</div>
    	<div class="guessrecommend" v-for = "item in recommend">
    		<img :src=item.product_image_url>
    		<div class="guess1">{{item.product_name}}</div>
    		<div class="guess2"><span >￥{{item.price/100}}</span>/<span>{{item.show_entity_name}}</span></div>
    	</div>
    </div>
    <div class="cart">
		<div><i class="iconfont" @click = "Cart()">&#xe624;</i></div>
		<div class="addcart" @click = "addCart()">加入购物车</div>
		<div class="computed" @click = "goCheck()">立即购买</div>
	</div>
   </div>
</div>
</template>


<script>
	import MyAjax from "./../md/MyAjax.js";
	import "./../scss/main.scss";
	import router from "./../router/router.js";
	export default{
		data(){
			return{
				imgs:[],
				name1:[],
				spec1:[],
				description1:[],
				price1:[],
				origin_price1:[],
				show_entity_name:[],
				show_entity_name1:[],
				modules1:[],
				restaurant_address1:[],
				restaurant_phone_numbers:[],
				
				contents:[],
				sub_title:[],
				text:[],
				lights:[],
				contents1:[],
				recommend:[]
			}
		},
		methods:{
			addCart(){
				var that = this;
				var proid = that.productID;
				console.log(proid);
				//var id = proid.split('&')[0];
				//console.log(id);
				var isgoods = localStorage.getItem('goods')	;	
				function isexit(currentobj,cartarr){
					for(var i in cartarr){
						if(currentobj.proid== cartarr[i].proid){
							return cartarr[i];
						}
					}
					return false;
				}
				var obj ={
						pic:that.imgs[0].img_url,
						name1:that.name1,
						spec1:that.spec1,
						description1:that.description1,
						price1:that.price1,
						proid:that.productID
					}
				var arr = [];
				if(isgoods == null){
					obj.num = 1;
					arr.push(obj);
					var str =JSON.stringify(arr);
					localStorage.setItem("goods",str);
					console.log(str);
				}else{
					var nowproarr = JSON.parse(localStorage.getItem('goods'))
					var exitflag = isexit(obj, nowproarr);
					if(exitflag){
						exitflag.num++;
						
					}else{
						obj.num =1;
						nowproarr.push(obj);
					}
					var nowproarrstr = JSON.stringify(nowproarr);
                    localStorage.setItem('goods', nowproarrstr);
                    console.log(nowproarrstr);
				}
				
				
			},
			Cart(){
				var that = this;
				that.$router.push({path:"/cart"});
			}
		},
		mounted(){
			var that = this;
			var proID = that.$route.query.proID;
			console.log(proID);
			var url="https://api.ricebook.com/product/info/product_detail.json?product_id="+proID;
			MyAjax.vueJson(url,function(data){
			
					console.log(data);
					that.name1 = data.basic.name;
					that.spec1 = data.basic.spec;
					that.description1 = data.basic.description;
					that.price1 = data.basic.price;
					that.origin_price1 = data.basic.origin_price;
					that.show_entity_name = data.basic.show_entity_name;
					
					that.modules1 = data.modules;
					that.restaurant_address1 = data.modules[0].data.restaurants[0].restaurant_address ;
					that.restaurant_phone_numbers = data.modules[0].data.restaurants[0].restaurant_phone_numbers[0];
					that.contents =data.modules[1].data.contents;
					
					that.lights =data.modules[2].data.lights;
					
					
					that.contents1 = data.modules[3].data.contents;
					
					that.recommend = data.modules[4].data.recommend;
					that.imgs=data.basic.product_images;
					console.log(data.basic.name);
					console.log(data.basic.product_images);
				},function(err){
					console.log(err)
				});
				  var swiper = new Swiper('.swiper-container', {
			        pagination: '.swiper-pagination',
			        observer:true
    });
			
		}
	}
</script>

<style scoped>
	.homecontent{
		width: 100%;
		height: 100%;
		overflow-y: auto;
	}
	.homecontent::-webkit-scrollbar {display: none;}
	 .swiper-container {
        width: 100%;
        height: 220px;
    }
    .swiper-slide img{
      width: 100%;
      height: 220px;
    }
    .con{
    	width: 90%;
    	height: 90%;
    	padding: 5%;
    	
    }
    .detitle{
    	width: 100%;
    	
    }
    .detitle span{
    	width: 100%;
    	line-height: 30px;
    	font-size: 25px;
    }
    .detitle2{
    	width: 100%;
    	
    	line-height: 40px;
    	font-size: 20px;
    }
    .deprice{
    	width: 100%;
    	height: 40px;
    	line-height: 40px;
    	border-bottom: 1px solid #ccc;
    	
    }
    .deprice span:nth-of-type(1){
    	font-size: 26px;
    	color: #FF6666;
    }
    .deprice span:nth-of-type(2){
    	font-size: 20px;
    	color: #FF6666;
    }
    .deprice span:nth-of-type(3){
    	font-size: 20px;
    	color: #ccc;
    	padding: 0 5%;
    	
    }
    .msg{
    	border-bottom: 1px solid #ccc;
    }
    .msgtitle{
    	text-align: center;
    	width: 100%;
    	height: 50px;
    	line-height: 50px;
    	font-size: 25px;
    	font-weight: 800;
    	border-bottom: 1px solid #ccc;
    }
    .didian,.num{
    	text-indent: 20px;
    	width: 100%;
    	height: 40px;
    	line-height: 40px;
    	font-size: 14px;
    	
    	border-bottom: 1px solid #ccc;
    }
    .menu{
    	border-bottom: 1px solid #ccc;
    }
    .menutitle,.nn,.usetitle,.guesstitle{
    	font-size: 25px;
    	width: 100%;
    	height: 50px;
    	line-height: 50px;
    	text-align: center;
    }
    .mainfood div,.usetext{
    	text-align: center;
    	width: 100%;
    	line-height: 30px;
    	font-size: 18px;
    	
    }
    .mainfood div:nth-child(1){
    	font-size: 20px;
    	font-weight: 600;
    	margin-top: 10px;
    }
    .bright{
    	width: 100%;
    }
    .bright img{
    	width: 100%;
    	height: 180px;
    	margin: 10px auto;
    }
    .brtitle{
    	width: 100%;
    	line-height: 30px;
    	font-size: 25px;
    }
    .brinfo{
    	width: 100%;
    	line-height: 30px;
    	font-size: 16px;
    }
    .guessrecommend{
    	width: 100%;
    	height: 120px;
    }
    .guessrecommend img{
    	display: block;
    	float: left;
    	width: 30%;
    	height: 100px;
    	margin-right: 5%;
    }
    .guess1{
    	
    	width: 65%;
    	float: left;
    	font-size: 12px;
    	line-height: 20px;
    	margin-bottom: 20px;
    }
    .guess2{
    	color: red;
    	font-size: 12px;
    }
    .cart{
    	position: fixed;
    	left: 0;
    	bottom: 0;
    	width: 100%;
    	height: 50px;
    }
	.iconfont{
		width: 20%;
		height: 50px;
		line-height: 50px;
		float: left;
		font-size: 30px;
		text-align:center ;
		
	}
	.addcart{
		width: 40%;
		height: 100%;
		float: left;
		font-size: 20px;
		text-align: center;
		line-height: 50px;
		background: yellow;
		color: #fff;
	}
	.computed{
		width: 40%;
		height: 100%;
		float: left;
		font-size: 20px;
		text-align: center;
		line-height: 50px;
		background: red;
		color: #fff;
	}
    
</style>