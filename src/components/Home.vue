<template>
	<div class="watch">
		<div v-if="load" class="loader-main">
			<div class="loader"></div>
		</div>
		<h1>1 Bitcoin'in Değeri Ne Kadar?</h1>
		<div class="watchbox">
			<p>
				<b>USD:</b> {{parseFloat(dolar).toFixed(2)}} 
				<i v-if="usddown" class="fa fa-arrow-down"></i>
				<i v-if="usdup" class="fa fa-arrow-up"></i>
			</p>
		</div>
		<div class="watchbox">
			<p>
				<b>TRY:</b> {{parseFloat(turklirasi).toFixed(2)}} 
				<i v-if="trydown" class="fa fa-arrow-down"></i>
				<i v-if="tryup" class="fa fa-arrow-up"></i>
			</p>
		</div>
		<div class="watchbox">
			<p><b>Son Güncelleme:</b> {{songuncelleme}}</p>
		</div>
		<p style="color:#aaa;">{{disclaimer}}</p>
	</div>
</template>
<script>
	export default{
		name: 'Home',
		data(){
			return{
				dolar: '',
				turklirasi: '',
				songuncelleme:'2017/11/19 ',
				usddown: '',
				usdup:'',
				trydown:'',
				tryup:'',
				load: true,
				disclaimer:''
			}
		},
		created(){

			this.hello();
		},
		methods:{
			getBitcoin(){
				this.$http.get('https://api.coindesk.com/v1/bpi/currentprice/TRY.json')
				.then((res) => {
					return res.data
				})
				.then((res) => {
						if(this.turklirasi > res.bpi.TRY['rate_float']){
							this.trydown = true;
							this.tryup = false;
						}
						if(this.turklirasi > res.bpi.TRY['rate_float']){
							this.trydown = false;
							this.tryup = true;
						}
						if(this.dolar > res.bpi.USD['rate_float']){
							this.usddown = true;
							this.usdup = false;
						}
						if(this.dolar > res.bpi.USD['rate_float']){
							this.usddown = false;
							this.usdup = true;
						}
					this.load = false;
					this.disclaimer = res.disclaimer;
					this.turklirasi = res.bpi.TRY['rate_float'];
					this.dolar = res.bpi.USD['rate_float'];
					var d = new Date(res.time['updatedISO']);
					var month = d.getMonth()+1
					this.songuncelleme = d.getDate() +'/'+ month +'/'+ d.getFullYear() +' '+ d.toLocaleTimeString();
				})
			},
			hello(){
				var _this = this;
				this.getBitcoin();
				setInterval(function(){
				_this.getBitcoin();
			}, 60000)
		}
	}
}



	
</script>


<style>
body{
	color:#fff;
	text-align:center;
}
h1{
	color:#000;
}
b{
	color:#000;
}
.watchbox{
	background-color: rgb(255, 38, 76);
	border-radius:5px;
	display:inline-block;
	box-shadow: 0px 0px 10px 1px #828282;
	padding:15px;
	margin:10px;
	min-width:25%;
}
.icon {
  display: inline-block;
  width: 1em;
  height: 1em;
  stroke-width: 0;
  stroke: currentColor;
  fill: currentColor;
}
.loader {	
  border: 16px solid #f3f3f3;
  border-radius: 50%;
  border-top: 16px solid #3498db;
  width: 120px;
  height: 120px;
  -webkit-animation: spin 2s linear infinite;
  animation: spin 2s linear infinite;
}
.loader-main{
	position:fixed;
	top:0px;
	left:0px;
	margin:0px;
	padding:0px;
	background-color:#000;
	width:100%;
	height:100vh;
	padding-right:45%;
	padding-left:45%;
	padding-top:40vh;
}

@-webkit-keyframes spin {
  0% { -webkit-transform: rotate(0deg); }
  100% { -webkit-transform: rotate(360deg); }
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}


	
</style>