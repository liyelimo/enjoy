<template>
	<div class="cartcontent">
		<div class="cartpro" v-for = "(item,index) in prolist">
			<img  :src=item.pic>
			<div class="cartright">
				<div>{{item.name1}}</div>
				<div>单价：{{item.price1/100}}</div>
				<div>
						<span class="tocountreduce" @click = "tocountreduce(item,index)">-</span>
						<span  class ="count" ref="count">{{item.num}}</span>
						<span class="tocountadd" @click = "tocountadd(item,index)">+</span>
						<span class = "todel" @click = "todel()">删除</span>
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
			<div class="zhifu" @click="zhifu()">
				去结算
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
				prolist:[],
				datalist:[]

			}
		},
		methods:{
			tocountreduce(item,index){
				var proList=this.prolist;	
				var num = proList[index].num;				
				if(num > 1){
					num--;
					proList[index].num=num;
				}
				var goods=JSON.parse(localStorage.getItem("goods"));
				goods[index].num=num;
				localStorage.setItem("goods",JSON.stringify(goods))				
			},
				
				
			
			tocountadd(item,index){
				var proList=this.prolist;
				var num=proList[index].num;
				num++;
				proList[index].num=num;
				var goods=JSON.parse(localStorage.getItem("goods"));
				goods[index].num=num;
				localStorage.setItem("goods",JSON.stringify(goods));
				this.proList=proList;		
			},

	
		todel(index){
//			var that = this;
//			that.prolist = JSON.parse(localStorage.getItem("goods"));
//				console.log(that.prolist);
var proList=this.prolist;
				proList.splice(index,1);
				this.proList=proList;
				var goods=JSON.parse(localStorage.getItem("goods"));
				if(proList.length==1){
					localStorage.removeItem("goods");
					
				}else{
					proList.splice(index,1);
					localStorage.setItem("goods",JSON.stringify(goods))
				}
		},
		zhifu(){
				this.$router.push("/buy")
			}
		
		},
		mounted(){
			var that = this;
			that.prolist = JSON.parse(localStorage.getItem("goods"));
			console.log(that.prolist);
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
		},
		updated(){
			var lists=localStorage.getItem("goods");
			var aaa=eval(lists);
			var qian=0;
			var danj=0;
			var totas=0;
			for(var item in aaa){
				qian=aaa[item].num;
				danj=aaa[item].price1;
				 totas+=qian*danj/100;
			}
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
	
.caiLike{
    width: 100%;
    }
  .caiLike  h1{
        width: 100%;
        font-size:1.0rem;
        text-align: center;
        line-height: 2rem;
    }
    
     .likeList{
         width: 50%;
         height: 12rem;
         float: left;
         font-size:0.6rem;  
         }      
      .likeList   img{
             width: 90%;           
             padding: 4%;
         }
       .likeList  p{
             padding: 4%;
         }
         
      .pricess{
             color:#f00;
         } 

.cartfooters{
    width: 100%;
    height: 2.5rem;
    line-height: 2.5rem;
    background: #fff;
    position: fixed;
    bottom: 0;
    left: 0;
    font-size: 0.6rem;
    border-top:1px solid #eee;
    }
    .point,
    .totalyun,
    .zhifu{
       width: 33.3%;
       float: left;      
       text-align: center;
      color: #f00;
    }
  
       #xunazho{
           color: #f00;
            
        }
    .zhifu{
        background: #f00;
     color: #fff;
     font-size: 0.8rem;
    }
</style>