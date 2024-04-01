<template>
	<view class="container">
		<view class="login-container">
			<view class="title">
				<view>
					<image src="../../static/logo.png" class="logo" style="width: 127px;height: 84px;"></image>
				</view>	
				<u--text text="智能车载系统" align="center" size="18"></u--text>
			</view>
			<br>
			
			<view  style="display: flex;">
				<view @click="tabValue=0">
					<u--text text="账号登录" size="18" :bold="!!!tabValue" :color="tabValue?'#303133':'#606266'"></u--text>
				</view>
				<view style="padding: 0 1rem;">
					<u--text text="|" size="18" ></u--text>
				</view>
				<view  @click="tabValue=1">
					<u--text text="手机登录" size="18" :bold="!!tabValue" :color="!tabValue?'#303133':'#606266'"></u--text>
				</view>
			</view>
			<view v-if="tabValue">
				<view  class="login-form">
					<u--input prefixIcon='phone' placeholder='请输入手机号:'  clearable border="bottom" prefixIconStyle='font-size:24px' v-model="phoneInfo.phone"></u--input>
					<br>
					<br>
					<u-row gutter="10">
						<u-col span="9">
							<u--input placeholder='请输入验证码:' ckearable border='bottom' prefixIconStyle='font-size:24px' v-model="phoneInfo.code"></u--input>
						</u-col>
						<u-col span="3">
							<u-button  @click="getCode()" type="primary" shape="circle" size="mini">获取验证码</u-button>
						</u-col>
					</u-row>
				</view>
				
			</view>
			<view v-if="!tabValue">
				<view  class="login-form">
					<u--input prefixIcon='account' placeholder='请输入账号:'  clearable border="bottom" prefixIconStyle='font-size:24px' v-model="loginInfo.username"></u--input>
					<br>
					<br>
					<u--input prefixIcon='lock' placeholder='请输入密码:' ckearable border='bottom' prefixIconStyle='font-size:24px' v-model="loginInfo.password"></u--input>
				</view>
			</view>
			<view>
				<u-button type="primary"  text="登录" @click="Login()" ></u-button>
				<br>
				<u-button type="primary"  plain text="注册" @click="goRegister"></u-button>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				loginInfo:{
					username:'',
					password:''
				},
				phoneInfo:{
					phone:'',
					code:''
				},
				tabValue:0,
			}
		},
		computed:{
			loginDist(){
				let loginInfo=new Array();
				if(!this.tabValue){
					loginInfo.push('/login',this.loginInfo)
				}else{
					loginInfo.push('/api/phone/login',this.phoneInfo)
				}
				return loginInfo;
			}
		},
		onLoad() {
			
		},
		methods: {
			Login(){
				this.$request.post(this.loginDist[0],this.loginDist[1])
				.then(res=>{
					if(res.code==200){
						uni.switchTab({
							url:'/pages/index/index',
							success: res => {},
							fail: () => {},
							complete: () => {}
						});
						uni.setStorageSync('token', res.token)
					}else{
						uni.showToast({
							title:res.msg,
							mask:true,
							icon:'error'
						})
					}
				})
				.catch(err=>{
					console.log(err);
					
				})
				
			},
			goRegister(){
				this.$request.post('/api/user/register',this.loginDist[1])
				.then(res=>{
					if(res.code==200){
						this.$request.requestShowToastSuccess('注册成功');
					}else{
						uni.showToast({
							title:res.msg,
							mask:true,
							icon:'error'
						})
					}
				})
				.catch(err=>{
					console.log(err);
					
				})
			},
			getCode(){
				this.$request.get(`/api/user/code?phone=${this.phoneInfo.phone}`)
				.then(res=>{
					if(res.code==200){
						console.log(res);
						this.code=res.msg
						this.$request.requestShowToastSuccess('验证码：'+res.msg);
					}else{
						this.$request.requestShowToastError(res.msg);
					}
				})
				.catch(err=>{
					this.$request.requestShowToastError(err);
				})
			}
		}
	}
</script>

<style lang="scss">
	@import url('/styles.scss');
	.login-container{
			text-align: center;
			font-size: 1.4rem;
			font-weight: 300;
			
			padding-top: 6rem;
			.login-form{
				padding: 1rem 0;
			}
		}
</style>
