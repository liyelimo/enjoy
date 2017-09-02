<template>
	<div class="cartcontent">
		<div class="cartpro" v-for = "item in prolist">
			<img  :src=item.pic>
			<div class="cartright">
				<div>{{item.name1}}</div>
				<!--<div>{{item.description1}}</div>-->
				<div>单价：{{item.price1/100}}</div>
				<div>
						<span class="tocountreduce" @click = "tocountreduce()">-</span>
						<span  class ="count" ref="count">{{item.num}}</span>
						<span class="tocountadd" @click = "tocountadd()">+</span>
						<span class = "todel" @click = "todel()">删除</span>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import "./../scss/main.scss";
	import router from "./../router/router.js";
	export default{
		data(){
			return{
				prolist:[]
			}
		},
		methods:{
			tocountreduce(){
				var arr = JSON.parse(localStorage.getItem("goods"));
				console.log(arr);
				var value = $(this.$refs.count).html();
				console.log(value)
				if(value > 1){
					value--;
					$(this.$refs.count).html(value);
				}
				var values = $(this.$refs.count).html();
				for(var item in arr){
					arr[item].num=values;
				}
				localStorage.setItem("goods",JSON.stringify(arr));
			},
			tocountadd(){
				var arr = JSON.parse(localStorage.getItem("goods"));
				console.log(arr);
//				var value = $(this.$refs.tocountreduce).html();
//				value++;
//				var values = $(this.$refs.tocountreduce).html();
//				for(var item in arr){
//					arr[item].num=values;
//				}
//				localStorage.setItem("goods",JSON.stringify(arr));
		},
		todel(){
			var arr = JSON.parse(localStorage.getItem("goods"));
				console.log(arr);
		}
		},
		mounted(){
			var that = this;
			var cartProduct = JSON.parse(localStorage.getItem("goods"));
			console.log(cartProduct);
			that.prolist = cartProduct;
			console.log(that.prolist)
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
		height: 130px;
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
		
	}
</style>