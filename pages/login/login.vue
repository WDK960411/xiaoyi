<template>
	<view class="content">
		<view class="logo">
			<image class="logo" src="../../static/img/xiaoyilogo.png" mode=""></image>
		</view>
		<view class="head-title">
			小医在诊用户登入
		</view>
		<view class="main">
			<view class="uni-form-item uni-column" >
				<input class="input phone" @input="Inputphone" v-model="phone" placeholder="请输入手机号" />
			</view>
			<view class="uni-form-item uni-column">
				<input class="input password" @input="Inputpassword" v-model="password" password type="text" placeholder="请输入密码" />
			</view>
			<view class="uni-form-item uni-column"  v-if="show">
				<input class="input verification" @input="Inputverification"  v-model="verification" type="text" placeholder="请输入图形验证码" />
				<view class="verification_img">
					<image :src="src" mode="" class="img"></image>
				</view>
			</view>
			<checkbox-group class="operation">
				<label>
					<view class="txt register" style="float: left;" @tap="register">
						免费注册
					</view>
					<view class="txt reset_password" style="float: right;" @tap="reset_password">
						忘记密码
					</view>
				</label>
			</checkbox-group>
			<view class="button">
				<button  class="enter" @tap="enter">登录</button>
			</view>
		</view>
		<view class="cut_off_wire">
			<view class="cut_off_txt">
				第三方登录
			</view>
		</view>
		<view class="thirdparty">
			<view class="box wechat" @tap="gowechat">
				<image class="logo wechat_logo" src="../../static/img/wechat.png" mode=""></image>
				<span class="txt">微信登录</span>
			</view>
			<view class="box Alipay" @tap="goAlipay">
				<image class="logo Alipay_logo" src="../../static/img/Alipay.png" mode=""></image>
				<span class="txt" style="margin-top: 15rpx;">支付宝登录</span>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		components: {
			
		},
		onLoad() {
			console.log('初始账号：13105311234','初始密码：123456')
		},
		data() {
			return {
				phone:'',
				password:'',
				show:false,
				verification:'',
				type:2,
				src:'../../static/img/verification.png'
			}
		},
		methods: {
			Inputphone:function(event) {
				this.phone = event.target.value
			},
			Inputpassword:function(event) {
				this.password = event.target.value
			},
			Inputverification:function(event) {
				this.verification = event.target.value
			},
			//免费注册
			register(){
				uni.reLaunch({
					url: './Model/register',
				});
			},
			//忘记密码
			reset_password(){
				uni.reLaunch({
					url: './Model/reset_password',
				});
			},
			//登入
			enter(){
				if(this.name === ''&&this.password === ''){
					uni.showToast({
						title: '请输入手机号和密码',
						icon: "none"
					});
				}else{
					
					if(this.show === false){
						let param = {
							mobile: this.phone,
							password:this.password,
							type:this.type
						};
						let qs = require("qs");
							param = qs.stringify(param);
						this.$request('/yl/login', {
							data: param,
							method:'POST',
							header: {
							    'custom-header': 'login', 
								'Content-Type': 'application/x-www-form-urlencoded'
							},
						}).then(res => {
							if(res.data.code === 200){
								uni.reLaunch({
									url: '../Home/index',
								});
								uni.setStorage({
									key: 'user_data',
									data: res.data.data,
									success:(()=> {
									   
									})
								});
							}else{
								uni.showToast({
									title: '手机号或密码输入错误',
									icon: "none"
								});
								this.show = true
							}
						})
						//请求测试
						// uni.request({
						//     url: 'http://192.168.0.225:8001/yl/login', 
						//     data: param,
						// 	method:'POST',
						//     header: {
						//         'custom-header': 'login', 
						// 		'Content-Type': 'application/x-www-form-urlencoded'
						//     },
						//     success: (res) => {
						// 		console.log(res.data.code);
						// 		uni.setStorage({
						// 		    key: 'user_data',
						// 		    data: res.data.data,
						// 		    success: function () {
								       
						// 		    }
						// 		});
						// 		if(res.data.code === 200){
						// 			uni.reLaunch({
						// 				url: '../Home/index',
						// 			});
						// 		}else{
						// 			uni.showToast({
						// 				title: '手机号或密码输入错误',
						// 				icon: "none"
						// 			});
						// 			this.show = true
						// 		}
						//     }
						// });
					}else{
						this.$request('/yl/login', {
							data: param,
							method:'POST',
							header: {
							    'custom-header': 'login', 
								'Content-Type': 'application/x-www-form-urlencoded'
							},
						}).then(res => {
							// 打印调用成功回调
							console.log(res)
						})
						// if(this.verification !== 'vwo7'){
						// 	uni.showToast({
						// 		title: '验证码输入错误',
						// 		icon: "none"
						// 	});
						// 	this.password = '',
						// 	this.verification = ''
						// }else{
						// 	if(this.phone !== 'admin'&&this.password !== '123wdk'){
						// 		this.phone = '',
						// 		this.password = '',
						// 		this.verification = ''
						// 	}else{
						// 		uni.reLaunch({
						// 			url: '../Home/index',
						// 		});
						// 	}
						// }
					}
				}
			},
			//支付宝登录
			goAlipay(){ 
				uni.showToast({
					title: '暂未开发',
					icon: "none"
				}); 
			},
			gowechat(){
				uni.showToast({
					title: '暂未开发',
					icon: "none"
				});
			}
		}
	}
</script>

<style lang="scss">
	//字体图标
	@import url("../../static/iconfont/iconfont.css");
	.content{
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		.logo{
			width: 180rpx;
			height: 180rpx;
			margin-top: 30rpx;
		}
		.head-title{
			width: 310rpx;
			height: 120rpx;
			margin-top: 90rpx;
			font-weight: 600;
			font-size: 39rpx;
		}
		.main{
			margin-top: 10rpx;
			width: 650rpx;
			// height: 550rpx;
			.operation{
				margin-top: 40rpx;
				.txt{
					font-size: 28rpx;
					margin-top: 6rpx;
					color:rgb(13,163,180);
					font-weight: 500;
				}
			}
			.input{
				height: 80rpx;
				border-bottom: 2rpx solid rgba(213,217,225,1);
				background-color: #FFF !important;
				.uni-input-input{
					background-color: #FFF !important;
				}
			}
			.verification{
				width: 50%;
			}
			.verification_img{
				width: 45%;
				height: 81rpx;
				// background: rgba(213,217,225,1);
				float: right;
				margin-top: -81rpx;
				.img{
					width: 100%;
					height: 100%;
					background-size: cover;
				}
			}
			.button{
				margin-top: 150rpx;
				.enter{
					width: 640rpx;
					background:#169bd5;
					color: #fff;
					font-size: 36rpx;
				}
			}
		}
		.cut_off_wire{
			width: 60%;
			height: 2rpx;
			background: #333333;
			margin: 80rpx 0rpx 0rpx 0rpx;
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;
			.cut_off_txt{
				width: 50%;
				height: 50rpx;
				background: #fff;
				text-align: center;
				font-size: 36rpx;
			}
		}
		.thirdparty{
			width: 600rpx;
			height: 250rpx;
			// margin-top: 10rpx ;
			.box{
				width: 40%;
				height: 100%;
				display: flex;
				flex-direction: column;
				align-items: center;
				justify-content: center;
			}
			.wechat{
				float: left;
				margin-left: 8%;
				.wechat_logo{
					width: 130rpx;
					height: 130rpx;
					float: left;
				}
			}
			.Alipay{
				float: right;
				margin-right: 8%;
				.Alipay_logo{
					width: 105rpx;
					height: 105rpx;
					float: left;
				}
			}
		}
	}
</style>
