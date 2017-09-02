H5-1703-闫峥 21:04:42
<template>
	<div class="cartcontent">
		<div class="cartpro" v-for = "item in prolist">
			<img  :src=item.pic>
			<div class="cartright">
				<div>{{item.name1}}</div>
				<div>单价：{{item.price1/100}}</div>
				<div>
						<span class="tocountreduce" @click = "tocountreduce()">-</span>
						<span  class ="count" ref="count">{{item.num}}</span>
						<span class="tocountadd" @click = "tocountadd()">+</span>
						<span class = "todel" @click = "todel()">移除</span>
				</div>
			</div>
		</div>
			<div class="caiLike">
		<h1>猜你喜欢</h1>	
			<div v-for="item in datalist" class="likeList">
				<img v-bind:src=item.product_image>
				<p>{{item.short_name}}</p>
				<p class="pricess">
					{{item.price/100}}元
					<span>{{item.storage_state}}</span>
					/{{item.show_entity_name}}
				</p>
			</div>
	
	</div>	
		<div class="cartfooters">
			<div class="point">
				全选
			</div>
			<div class="totalyun">
				合计:				
				<span  class="allMony">
					0 元						
				</span>				
			</div>
			<div class="zhifu">
				去结算
			</div>
		</div>	
	</div>
</template>

<script>
	import MyAjax from "./../md/MyAjax.js";
	import "./../scss/cart.scss";
	import router from "./../router/router.js";
	export default{
		data(){
			return{
				prolist:[],
				datalist:[]
			}
		},
		methods:{			
			tocountreduce(){
				var that = this;
				var prolist = JSON.parse(localStorage.getItem("goods"));
				var value = $(this.$refs.count).html();
				if(value > 1){
					value--;
					$(this.$refs.count).html(value);
				}
					var values = $(this.$refs.count).html();
					var pris="";
				for(var i in prolist){
					prolist[i].num= values;
					pris=prolist[i].price1/100;
				}	
				var money=pris*values;
					$(".allMony").html(pris*values)
				localStorage.setItem("goods",JSON.stringify(prolist))
			},
			tocountadd(){
				var that = this;
				var prolists = JSON.parse(localStorage.getItem("goods"));
				var value = $(that.$refs.count).html();
				++value; 									
				var val = $(that.$refs.count).html(value);
				var ovalue=val[0].innerHTML;
			var pri="";
				for(var i in prolists){
					prolists[i].num=ovalue;	
					pri=prolists[i].price1/100;
				}
				$(".allMony").html(pri*ovalue)
				localStorage.setItem("goods",JSON.stringify(prolists))
		},
		todel(){
			var index=$(this).parent().parent().index();
				var name=JSON.parse(localStorage.getItem("goods"))
				name.splice(index,1);			
				$(".cartcontent").eq(index).css({display:"none"});
				localStorage.setItem("goods",JSON.stringify(name));
		}
		},
		mounted(){
			var that = this;
			that.prolist = JSON.parse(localStorage.getItem("goods"));												
			var url="https://api.ricebook.com/3/enjoy_product/cart_recommend_product.json?city_id=1";
			MyAjax.vueJson(url,function(data){
				that.datalist=data.content;
			},function(err){
				console.log(err)
			});
			var lists=localStorage.getItem("goods");
			var aaa=eval(lists);
			var qian="";
			var danj="";
			for(var item in aaa){
				qian=aaa[item].num;
				danj=aaa[item].price1;
			}
			var totas=qian*danj/100;
			$(".allMony").html(totas)
		}
	}
</script>


<style scoped>
	.cartcontent{
		width: 90%;
		height: 100%;
		padding: 5%;
	}
	.cartpro{
		width: 100%;
		height: 150px;
		
	}
	.cartpro img{
		display: block;
		float: left;
		width: 30%;
		height: 90px;
		margin-right: 10%;
	}
	.cartright{
		width: 60%;
		height: 130px;
		float: left;
	}
	.cartright div:nth-of-type(1){
		width: 100%;
		font-size: 12px;
		white-space: nowrap;
		text-overflow: ellipsis;
		overflow: hidden;
	}
	.cartright div:nth-of-type(2){
		width: 100%;
		font-size: 12px;
		color: #f66;
		line-height: 30px;
		margin-bottom: 30px;
	}
	.cartright div:nth-of-type(3){
		width: 100%;
		font-size: 12px;
		color: #f66;
	}
	.tocountreduce{
		width: 15%;
		height: 30px;
		display: inline-block;
		background-color: #ccc;
		text-align: center;
		line-height: 30px;
	}
	.count{
		width: 15%;
		height: 30px;
		display: inline-block;
		text-align: center;
		line-height: 30px;
		color: #f66;
	}
	.tocountadd{
		width: 15%;
		height: 30px;
		display: inline-block;
		background-color: #ccc;
		text-align: center;
		line-height: 30px;
		margin-right: 10%;
	}
	.todel{
		width: 20%;
		height: 30px;
		display: inline-block;
		text-align: center;
		line-height: 30px;
		color: #ccc;
		font-size: 12px;
	}
</style>
